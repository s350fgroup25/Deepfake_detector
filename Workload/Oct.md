## 8/10/2025
- Install Raspberry Pi 5
- 16GB 1TB (Raspberry Pi5)
- 32GB (SD card)
- Run model (cal time) 
## 11/10/2025
- Create /test
- python 
  - 加載音訊波形。
  - 使用特徵提取器對其進行預處理。
  - 透過模型運行處理後的輸入以獲得預測。
  - 將預測與協議中的真實標籤進行比較。
- model_inference.py
  - 負責完整的音訊讀取、特徵處理和模型推論，確保模型接收的輸入正確，並回傳預測結果與推論時間。
- run_test.py
  - 負責從 protocol 讀取真實標籤，隨機抽音檔執行推論並比較。
- audio_utils.py
  - 提供從資料夾與 protocol 檔抓取音訊路徑與對應真實標籤的工具函式
- check_audio_length.py
  - 檢查音訊長度 
- convert_protocol.py
  - 轉成模型可讀 protocol，且同時複製重命名音訊
## 21/10/2025
- /program/test.py
  
      print(f"Sample {i+1}:")
      print(f" file_name: {fname}")
      print(f" predicted_label: {pred}")
      print(f" ground_truth_label: {gt}")
      print(f" correct: {correct}")
      print(f" runtime_ms: {runtime_ms:.3f}")
      print(f" audio_duration_ms: {duration_ms}")
      
      print(f"Total samples evaluated: {min_len}")
      print(f"Total evaluation time (s): {(end_eval - start_eval):.3f}")
- /program/evla-sentence.py
  - max samles = 1 
- /program/single_evaluate.py
  - wav type audio flle
 
## 29/10/2025
- test_all.py
  - Output all print statements to test_output.txt:
      - Uses Python’s redirect_stdout to write printed output into a file.
- test_10.py
  - Randomly select 10 audio samples:
      - Modifies the dataset so only 10 randomly chosen samples are processed (if more than 10 samples available).
- can run in server

## 30/10/2025
- try to start up the enverioment in raspberry pi
- packing all required file in zip
  - asvpoof2019.zip
    - /datasets/models/wavlm-epoch50/
    - /datasets/LA
        - /ASVspoof2019_LA_cm_protocols
        - /ASVspoof2019_LA_eval
    - /program
    - /requirements.txt
    - 目錄
    
          ├── test.py
          ├── wavlm-epoch50.safetensors
          ├── dataset_sentence.py
          ├── model_sentence1.py
          ├── eer1.py
          └── /home/carmen/asvspoof/datasets/LA/
              ├── ASVspoof2019_LA_eval
              │     └── flac/
              └── ASVspoof2019_LA_cm_protocols
                    └── ASVspoof2019.LA.cm.eval.trl.txt
  
- SHH and VNC raspberry pi 5 (done)
  - import the testing enviroment (fail)
  - testing python 3.13 and python 3.11 

