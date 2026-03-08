### part 1  evaluation
- testing in asvspoof 2021
- radnom choose data in 200d data in LA and DF , each 100 T/F
- run in raspberry pi 5 , for testing performance , eer and auc

### part 2 Android 
- Research android , list out how i have to do . 

### Research 
- Android 上沒辦法直接跑 Python，也沒辦法直接用 transformers 下載 HuggingFace 模型；必須先在電腦上把模型轉成 TorchScript 或 ExecuTorch，再放進 App 裡用 Java/Kotlin 調用
- 建議：使用 PyTorch Mobile / ExecuTorch 跑 .pt / .pte 模型
- 音訊前處理（讀 wav、做特徵）也建議事先寫在模型裡，這樣 Android 端只需要餵 raw waveform 或簡單 tensor。你目前在 Python 用 torchaudio + Wav2Vec2FeatureExtractor，這兩個在 Android 端都沒有現成版本，所以要「封裝進模型」或改成自己在 Python 裡寫 feature extraction。
- 在 Python 這邊做：讀檔 → 正規化 → feature extraction → 丟進模型 → 把 feature extraction 也包進 TorchScript 模型裡。

## 1/3/2026
- (done) download asvspoof 2021 dataset 
