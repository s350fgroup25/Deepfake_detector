Research 
- Android 上沒辦法直接跑 Python，也沒辦法直接用 transformers 下載 HuggingFace 模型；必須先在電腦上把模型轉成 TorchScript 或 ExecuTorch，再放進 App 裡用 Java/Kotlin 調用
- 建議：使用 PyTorch Mobile / ExecuTorch 跑 .pt / .pte 模型
- 音訊前處理（讀 wav、做特徵）也建議事先寫在模型裡，這樣 Android 端只需要餵 raw waveform 或簡單 tensor。你目前在 Python 用 torchaudio + Wav2Vec2FeatureExtractor，這兩個在 Android 端都沒有現成版本，所以要「封裝進模型」或改成自己在 Python 裡寫 feature extraction。
- 在 Python 這邊做：讀檔 → 正規化 → feature extraction → 丟進模型 → 把 feature extraction 也包進 TorchScript 模型裡。

現有系統主要有三個能力：
- 上傳或錄音 → 轉成 16kHz mono wav (/upload, /convert)
- 用 Wav2Vec2FeatureExtractor + HFReadyModel 做 inference (/analyze)
- 連續錄音，每 4 秒切一段送去分析 (/realtime_continuous + realtime_continuous.js)

在 Android 上要達到同樣功能，有兩種主流做法：
1. 輕改法：Android 只做 UI，模型還是跑在伺服器（Raspberry Pi 或雲端)
2. 完全本地：把模型轉成 On‑device 格式（TFLite / ONNX / GGML），用 Android 原生計算

Need : 
- 把現在的 Wav2Vec2 + HFReadyModel（PyTorch + safetensors）轉成 ONNX。
- 再從 ONNX 轉成 TFLite（或直接在 Android 上跑 ONNX Runtime）

Task : 
1. 把 HFReadyModel 改成可匯出 ONNX 的版本
- create export_onnx.py 產生 voice_auth.onnx
  ✅ Exported to voice_auth.onnx (1205.5 MB)
  🎉 ONNX export completed successfully!
  =>在 Python 這邊用 ONNX Runtime 測試一次輸入相同 waveform，確定 logits 跟原本 PyTorch 模型差不多（這一段我下一回合可以幫你寫完整測試腳本）。
  - test_onnx.py
  (venv) carmen@hkmu-1080ti:~/asvspoof/program$ python validate_onnx.py
  ✅ ONNX 結構正確！
  
  📊 Graph nodes: 1796
  📊 Graph outputs: 1
     Output: logits, shape=[dim_param: "batch"
  , dim_value: 2
  ]
  ✅ Model is valid!
  => 把 voice_auth.onnx 放進 Android app 的 app/src/main/assets/，讓手機可以讀。
2. 建一個 Android「Convert + 推論」專案骨架\
  => 加 ONNX Runtime 依賴:打開 app/build.gradle（Module: app），在 dependencies
3. 設計「Convert + AI 分析」UI（res/layout/activity_main.xm）
    按鈕：選檔
    檔案資訊 TextView
    按鈕：本地推論
    顯示結果：REAL / FAKE + 機率
4. MainActivity.kt – 完整骨架（含讀 assets + 簡單 ONNX call）
   => app/src/main/java/com/example/voiceauthlocal/MainActivity.kt

<img width="1390" height="1031" alt="image" src="https://github.com/user-attachments/assets/4d325797-fcd6-4bc9-92ea-8d8816d25218" />
<img width="1920" height="1033" alt="image" src="https://github.com/user-attachments/assets/d9d8313d-c83b-43ce-9c4c-1eeb43566fcd" />
