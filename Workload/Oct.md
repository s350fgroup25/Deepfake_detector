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
