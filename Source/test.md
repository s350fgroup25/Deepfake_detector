## /test
### 1. 部署與運行說明：
- 安裝必須 Python 套件：
  
      pip install torch torchaudio transformers safetensors numpy
### 2. 資料與模型
- 將你的 wavlm-epoch50.safetensors 模型權重放在 /program 內。
- 確認 ASVspoof2019 LA 評測集音訊資料與 protocol 檔案路徑設定正確。
### 3. 程式碼結構
      /asvspoof/
        ├── program/
        │    ├── model_sentence1.py
        │    ├── wavlm-epoch50.safetensors
        ├── test/
        │    ├── audio_utils.py      # 上述提供的工具函式檔
        │    ├── model_inference.py  # 前面給的推論執行檔
        │    ├── run_test.py         # 測試主程式
        │    ├── check_audio_length.py # 檢查音訊檔長度
### 4. 執行測試
- 進入 /asvspoof/test 資料夾，啟動你的 Python 環境後執行：
  
       python run_test.py
- 此程式會依照 protocol 隨機挑選音訊，對每個音訊推論真偽標籤並輸出結果與效能。

### 5. 檢查音訊檔長度
      check_audio_length.py

## run_test.py
      python run_test.py
  
- Output 
  - File: xxxx.flac
  - Ground Truth: spoof (False)
  - Predicted: spoof (False)
  - Correct: True
  - Runtime: 0.020 seconds

## check_audio_length.py
      python check_audio_length.py /../../xxx.flac
  
- Output 
  - Audio file: xxx.flac
  - Sample rate: 16000 Hz
  - Number of samples: 47919
  - Duration: 2.99 seconds
