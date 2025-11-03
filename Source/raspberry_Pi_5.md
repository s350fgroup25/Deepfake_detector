## SSH
  - ssh carmen@192.169.1.149 
  -  need same wifi 
## VNC 
  - 192.168.1.149:9

# Solution of install testing environment
## 方案一：使用預先編譯的 ARM64 架構 PyTorch
- 1. 將安裝僅 CPU版本，避免 CUDA 依賴項並節省空間，同時仍允許推理。
  - pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cpu
- 2. 安裝 Hugging Face 和 safetensors 庫：
  - pip install transformers safetensors numpy soundfile
- 3. run test_all.py

## 方案二：輕量級替代方案（如果 PyTorch 仍然失敗）
- 1.  將訓練好的.safetensors模型轉換為 ONNX 或 TorchScript
- 2.  在更強大的機器上使用ONNX Runtime運行推理
  - 在已安裝 PyTorch 的桌上型電腦上 (jupyterlab)
  
          import torch
          from model_sentence1 import Model
          from safetensors.torch import load_file
          
          model = Model(model_type='wavlm', device='cpu')
          state_dict = load_file('wavlm-epoch50.safetensors')
          model.load_state_dict(state_dict)
          model.eval()
          
          dummy_input = torch.randn(1, 16000)  # example 1s of audio
          torch.onnx.export(model, dummy_input, "wavlm_model.onnx", opset_version=12)
- 3. 將 ONNX 檔案複製到您的 Raspberry Pi
- 4. 安裝 ONNX 執行時間

      pip install onnxruntime onnx numpy soundfile
- 5. 修改test_all.py 檔案以使用

          import onnxruntime as ort
          # load model once
          ort_session = ort.InferenceSession("wavlm_model.onnx")
          
          # run inference instead of `trainer.predict(...)`
          outputs = ort_session.run(None, {"input": input_array.astype(np.float32)})
     
## 方案3 ：：僅運行推理部分
- 如果你已經實現了部分 PyTorch，但想要跳過整個 Hugging FaceTrainer邏輯
- 避免初始化TrainingArguments，並且Trainer保持最少的依賴項處於活動狀態
- 
            model.eval()
            with torch.no_grad():
                for i, batch in enumerate(eval_dataset):
                    inputs = batch["input_values"].to(device)
                    labels = batch["label"]
                    logits = model(inputs)
                    pred = torch.argmax(logits, dim=-1)
                    ...
## 概括
- 除非有非常特殊的需求，否則無需從原始碼建立 PyTorch 。
- 使用預先建置的 PyTorch wheels（pip 安裝）是啟用偵測的實用且簡單的方法。
- 修復 Python 系統相依性（例如 bz2），以避免使用過程中出現匯入錯誤。
- 對於像 Raspberry Pi 這樣資源受限的設備，僅支援 CPU 的 PyTorch wheels 或 ONNX 執行時間環境就足夠了。

## 專為內存有限的樹莓派量身定制的更簡便方法 
- 如果可能，請使用 PyTorch預先建置的輕量級版本（例如，適用於 ARM64 的 PyTorch 1.10 或 2.0）。
- 透過直接運行推理來避免使用繁重的 Transformer Trainer 和 TrainingArguments 機制。
- 使用最少的依賴項：torch、transformers 特徵提取器、safetensors 用於模型載入。
- 批次大小應保持較小（例如，1 或 2），以減少記憶體使用量。
- 由於樹莓派可能不支援 CUDA，請使用 CPU 裝置並停用 GPU 特定的環境變數。
- 簡化的推理範例（基於您的test_all.py核心邏輯 (re-write  code) -- test_less.py
- 
