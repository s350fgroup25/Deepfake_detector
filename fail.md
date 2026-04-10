# VeriVox Android 部署完整失败报告

## 一、概述

本项目尝试将 VeriVox 音频深度伪造检测系统从 Raspberry Pi 迁移到 Android 平台，分别尝试了 **Termux + PyTorch** 和 **原生 Android + ONNX Runtime** 两种技术方案。

**最终结论：两种方案均失败，无法在 Android 设备上获得正确的推理结果。**

---

## 二、硬件环境

| 项目 | 规格 |
|------|------|
| 设备型号 | Xiaomi Pad 6 Max 14 |
| 处理器 | Snapdragon 8+ Gen 1 |
| 内存 | 12GB + 3GB 扩展 |
| 存储 | 256GB |
| 操作系统 | Android 13 |
| Termux 版本 | 0.118.0 (F-Droid) |

---

## 三、方案一：Termux + PyTorch 部署

### 3.1 成功安装的组件

#### 系统包（✅ 成功）

| 包名 | 版本 | 状态 |
|------|------|------|
| Python | 3.13 | ✅ |
| ffmpeg | latest | ✅ |
| libsndfile | 1.2.2-2 | ✅ |
| cmake | 4.3.1 | ✅ |
| ninja | 1.13.2 | ✅ |
| openssh | latest | ✅ |

#### Python 包（✅ 成功安装）

| 包名 | 版本 | 状态 |
|------|------|------|
| torch | 2.11.0 | ✅ |
| transformers | 5.5.3 | ✅ |
| safetensors | 0.7.0 | ✅ |
| soundfile | 0.13.1 | ✅ |
| Flask | 3.1.3 | ✅ |
| numpy | 2.4.3 | ✅ |

#### 安装失败的包（❌ 失败）

| 包名 | 失败原因 |
|------|---------|
| scipy | 缺少 Fortran 编译器 (gfortran) |
| librosa | 依赖 scipy |
| torchaudio | ARM64 架构无预编译包 |

### 3.2 核心问题：推理结果不一致

#### 问题描述

| 平台 | 音频 | 真实概率 | 判断结果 |
|------|------|---------|---------|
| Raspberry Pi (基准) | record_sample2.wav | 0.3-0.4 (30-40%) | FAKE ✅ |
| Termux (Android) | record_sample2.wav | 0.99+ (99%+) | REAL ❌ |

#### 测试数据对比

| 测试次数 | 树莓派结果 | Termux 结果 | 一致性 |
|---------|-----------|-------------|--------|
| 第1次 | 0.35 (FAKE) | 0.9924 (REAL) | ❌ |
| 第2次 | 0.38 (FAKE) | 0.9963 (REAL) | ❌ |
| 第3次 | 0.33 (FAKE) | 0.9982 (REAL) | ❌ |
| 第4次 | 0.36 (FAKE) | 0.9981 (REAL) | ❌ |
| 第5次 | 0.34 (FAKE) | 0.9975 (REAL) | ❌ |

#### 错误日志

```
Wav2Vec2Model LOAD REPORT from: ./wav2vec2-xls-r-300m
Key                          | Status
-----------------------------+----------
project_q.weight             | UNEXPECTED
project_hid.bias             | UNEXPECTED
quantizer.weight_proj.bias   | UNEXPECTED
project_hid.weight           | UNEXPECTED
project_q.bias               | UNEXPECTED
quantizer.codevectors        | UNEXPECTED
quantizer.weight_proj.weight | UNEXPECTED

缺失的键（来自预训练模型）: 422 个
意外的键: 7 个
```

#### HuggingFace TLS 下载错误

```
thread 'hf-xet-0' panicked at .../rustls-platform-verifier/src/android.rs:94:10:
Expect rustls-platform-verifier to be initialized
```

### 3.3 尝试过的解决方案及结果

| 方案 | 描述 | 结果 |
|------|------|------|
| 方案1 | 使用 `strict=False` 加载 | ⚠️ 部分成功，但结果仍错误 |
| 方案2 | 从树莓派复制完整模型文件夹 | ⚠️ 文件太大（~1.2GB），传输失败 |
| 方案3 | 在线下载 Wav2Vec2 | ❌ TLS 错误，无法下载 |
| 方案4 | 固定随机种子 + eval 模式 | ❌ 仍无法复现树莓派结果 |

---

## 四、方案二：原生 Android + ONNX Runtime 部署

### 4.1 技术方案

| 项目 | 规格 |
|------|------|
| 开发环境 | Android Studio (Kotlin + Jetpack Compose) |
| ONNX Runtime 版本 | 1.19.0 |
| 模型格式 | ONNX (从 PyTorch 导出) |
| 输入形状 | `[1, 77824]` (固定长度) |

### 4.2 依赖配置

```kotlin
dependencies {
    implementation("com.microsoft.onnxruntime:onnxruntime-android:1.19.0")
}
```

### 4.3 错误现象

#### 日志输出

```
=== START ===
✓ Environment OK
Model file: .../model.onnx, size: 1.2 GB
✓ Session created!
WAV size: 311296 bytes, samples: 77824
Raw audio: 77824 samples
✓ Normalized: 77824, mean: 0.0012, std: 0.1234
✓ Input tensor: [1, 77824]
Running inference...
✓ Inference complete: 2 logits
```

#### 推理结果（错误）

```
logits length: 2          ✅ 正常（二元分类）
max logit: NaN            ❌ 致命错误
argmax: -1                ❌ 致命错误
```

### 4.4 错误含义

| 输出值 | 含义 | 状态 |
|--------|------|------|
| `logits length: 2` | 模型输出 2 个值（Real/Fake） | ✅ 正常 |
| `max logit: NaN` | 所有输出值都是 NaN（非数字） | ❌ 推理失败 |
| `argmax: -1` | 无法找到最大值（因为全是 NaN） | ❌ 推理失败 |

### 4.5 核心代码问题

```kotlin
val logits = outputTensor.floatBuffer.array()
// ❌ floatBuffer.array() 没有正确提取浮点数，导致空数组或无效数据
```

### 4.6 可能的技术原因

| 原因 | 说明 | 可能性 |
|------|------|--------|
| **ByteOrder 不匹配** | Android 设备的字节序与 ONNX 模型期望不一致 | 🔴 高 |
| **内存对齐问题** | `ByteBuffer.allocateDirect()` 分配的内存未正确对齐 | 🟡 中 |
| **模型输入格式错误** | ONNX 模型期望的输入格式与 Android 端提供的不一致 | 🟡 中 |

### 4.7 尝试过的解决方案

| 尝试 | 方案 | 结果 |
|------|------|------|
| 1 | 使用 `ByteBuffer.order(ByteOrder.nativeOrder())` | ❌ 无效 |
| 2 | 使用 `ByteBuffer.order(ByteOrder.LITTLE_ENDIAN)` | ❌ 无效 |
| 3 | 直接使用 `floatBuffer.get()` 逐元素读取 | ❌ 仍为 NaN |
| 4 | 重新导出 ONNX 模型 (opset 16) | ❌ 无效 |
| 5 | 降级 ONNX Runtime 版本到 1.15.0 | ❌ 无效 |
| 6 | 升级 ONNX Runtime 版本到 1.20.0 | ❌ 无效 |

---

## 五、两种方案对比总结

| 方案 | 模型格式 | 推理结果 | 状态 |
|------|---------|---------|------|
| Termux + PyTorch | `.safetensors` | 错误 (0.99 vs 0.35) | ❌ 失败 |
| Android + ONNX Runtime | `.onnx` | NaN | ❌ 失败 |
| Raspberry Pi + PyTorch | `.safetensors` | 正确 (0.3-0.4) | ✅ 成功 |

---

## 六、失败原因总结

### 6.1 Termux 方案失败原因

| 原因 | 说明 |
|------|------|
| **权重加载不完整** | `model.safetensors` 包含 468 个键，但只有 46 个匹配当前模型定义 |
| **Wav2Vec2 模型无法正确加载** | 本地文件夹不完整，在线下载因 TLS 错误失败 |
| **UNEXPECTED 键** | 加载时出现 7 个 UNEXPECTED 键（project_q, quantizer 等） |
| **随机初始化** | 不匹配的权重被忽略，模型部分随机初始化 |

### 6.2 Android ONNX 方案失败原因

| 原因 | 说明 |
|------|------|
| **`floatBuffer.array()` 提取失败** | 无法正确提取浮点数，导致空数组或无效数据 |
| **ByteOrder 不匹配** | Android 设备的字节序与 ONNX 模型期望不一致 |
| **内存对齐问题** | `ByteBuffer.allocateDirect()` 分配的内存未正确对齐 |

---

## 七、最终结论

### 7.1 部署状态

| 方案 | 部署状态 | 根本原因 |
|------|---------|---------|
| Termux + PyTorch | ❌ 失败 | 权重不匹配 + Wav2Vec2 无法正确加载 |
| Android + ONNX Runtime | ❌ 失败 | floatBuffer.array() 提取失败，输出 NaN |

### 7.2 成功运行的唯一平台

| 平台 | 状态 |
|------|------|
| Raspberry Pi 5 | ✅ 成功运行 |

### 7.3 建议的替代方案

| 方案 | 描述 | 可行性 |
|------|------|--------|
| 方案A | 使用 ONNX Runtime 但修复 ByteOrder 问题 | ⚠️ 需进一步调试 |
| 方案B | 将模型转换为 TensorFlow Lite | ⚠️ 需测试 |
| 方案C | 放弃 Android 部署，专注 Web 接口 | ✅ 可行 |
| 方案D | 使用 Web 前端 + 后端服务器架构 | ✅ 可行 |

---

## 八、附录

### 8.1 Termux 模型加载错误日志

```
❌ Model loading failed: Repo id must be in the form 'repo_name' or 'namespace/repo_name': '/home/carmen/asvspoof/program/wav2vec2-xls-r-300m-feature'. Use `repo_type` argument if needed.
```

### 8.2 ONNX Runtime 推理代码关键部分

```kotlin
val results = session!!.run(mapOf("input_values" to inputTensor!!))
val outputTensor = results.toList().first().value as? OnnxTensor
val logits = outputTensor.floatBuffer.array()  // ❌ 返回 NaN
```

### 8.3 验证方法（尝试诊断）

```kotlin
// 检查 Buffer 内容
val floatBuffer = outputTensor.floatBuffer
Log.d("ONNXTest", "Buffer remaining: ${floatBuffer.remaining()}")
Log.d("ONNXTest", "Buffer hasArray: ${floatBuffer.hasArray()}")

// 手动读取每个值
val logits = FloatArray(floatBuffer.remaining())
for (i in 0 until logits.size) {
    logits[i] = floatBuffer.get(i)
    Log.d("ONNXTest", "logits[$i] = ${logits[i]}")
}
// 输出仍然为 NaN
```

---

*报告完成日期: 2026年4月*
*项目: VeriVox Audio Deepfake Detector*
