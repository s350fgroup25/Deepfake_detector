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
 

## import other audio in other datasets 
1. 新建音訊資料夾，放入原始音訊

       mkdir my_audio
       mkdir my_output/audio

2.製作清單（list.txt）紀錄音訊的檔案名與真假（T/F）

      nano my_audio/list.txt
3.用 convert_protocol.py 轉成模型可讀 protocol，且同時複製重命名音訊

      nano test/convert_protocol.py

      python convert_protocol.py /home/carmen/my_audio/list.txt /home/carmen/my_audio /home/carmen/my_output/protocol.txt /home/carmen/my_output/audio
4.修改 run_test.py 指向輸出目錄與 protocol

        from audio_utils import select_random_audio_files
        from model_inference import AudioSpoofDetector
        
        def main():
            AUDIO_FOLDER = '/home/carmen/my_output/audio'
            PROTOCOL_PATH = '/home/carmen/my_output/protocol.txt'
            MODEL_PATH = '/program/wavlm-epoch50.safetensors'
            NUM_SAMPLES = 5  # 或您想測試的數量
        
            selected_files = select_random_audio_files(AUDIO_FOLDER, PROTOCOL_PATH, NUM_SAMPLES)
            detector = AudioSpoofDetector(MODEL_PATH)
        
            for filepath, true_label in selected_files:
                pred_label, runtime = detector.predict(filepath)
                true_str = 'bonafide (True)' if true_label else 'spoof (False)'
                correct = (pred_label == true_str)
                print(f"File: {filepath}")
                print(f"Ground Truth: {true_str}")
                print(f"Predicted: {pred_label}")
                print(f"Correct: {correct}")
                print(f"Runtime: {runtime:.3f} seconds\n")
        
        if __name__ == "__main__":
            main()

5. 執行 run_test.py 進行模型測試
   
       python run_test.py 
