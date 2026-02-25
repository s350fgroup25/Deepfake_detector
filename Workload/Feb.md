## 1/2/2026
#### Fake Biden robocall : Clip0
- https://soundcloud.com/user-429524614/fake-joe-biden-robocall-nh
#### Test:Clip1-5
- https://www.theguardian.com/us-news/2024/feb/26/ai-deepfake-quiz-spot-the-sham-audio-of-trump-and-biden
#### Tools:
- using :https://cloudconvert.com/mp4-to-mp3
- Parrot AI :https://www.tryparrotai.com/ai-voice-generator/donald-trump 
- ai-voice-generator :
  - noiz.ai : https://noiz.ai/
  - https://elevenlabs.io/
- Voice Changer : https://tw.imyfone.com/voice-changer/demos/
#### video to audio 
- create a new web page for mp4 to mp3
- /video2audio
#### data 
- 🔍 Analysis: convert_Clip0_Fake_Biden_robocall_1770543366.wav → Real: 0.0069, Fake: 0.9931
- 🔍 Analysis: convert_Clip1_Real_Biden_1770543386.wav → Real: 0.3105, Fake: 0.6895
- 🔍 Analysis: convert_Clip2_Real_trump_1770543537.wav → Real: 0.0396, Fake: 0.9604 (noise)
- 🔍 Analysis: convert_Clip3_Real_Biden_1770543577.wav → Real: 0.0025, Fake: 0.9975 (noise)
- 🔍 Analysis: convert_Clip4_Fake_Biden_1770543645.wav → Real: 0.0525, Fake: 0.9475 
- 🔍 Analysis: convert_Clip5_Fake_trump_1770543692.wav → Real: 0.0327, Fake: 0.9673
- 🔍 Analysis: convert_Clip6_Fake_trump_1770543742.wav → Real: 0.0000, Fake: 1.0000
- 🔍 Analysis: convert_Clip7_AI_Book_1770543766.wav → Real: 0.0175, Fake: 0.9825
- 🔍 Analysis: convert_Clip8_HP_1770543794.wav → Real: 0.4548, Fake: 0.5452
- 🔍 Analysis: convert_clip8_cut_1770544539.wav → Real: 0.0012, Fake: 0.9988
- 🔍 Analysis: convert_Clip9_HP_1770543824.wav → Real: 0.0160, Fake: 0.9840
- 🔍 Analysis: convert_Clip10_HP_1770543853.wav → Real: 0.0181, Fake: 0.9819

//video (problem , no video is real)
- 🔍 Analysis: 2026-02-08_15-35-38_1770545989.wav → Real: 0.0126, Fake: 0.9874
- 🔍 Analysis: 2026-02-08_15-38-11_1770546042.wav → Real: 0.0130, Fake: 0.9870
- 🔍 Analysis: 2026-02-08_19-42-45_1770551072.wav → Real: 0.0003, Fake: 0.9997
- 🔍 Analysis: 2026-02-08_19-41-56_1770551139.wav → Real: 0.0001, Fake: 0.9999
- 🔍 Analysis: 2026-02-08_19-42-20_1770551199.wav → Real: 0.0009, Fake: 0.9991

// record 
- cantonese
- 🔍 Analysis: recording_1770550065125.wav → Real: 0.0720, Fake: 0.9280
- 🔍 Analysis: recording_1770550329537.wav → Real: 0.0803, Fake: 0.9197
- 🔍 Analysis: recording_1770550522208.wav → Real: 0.2881, Fake: 0.7119
- 🔍 Analysis: recording_1770550610053.wav → Real: 0.3049, Fake: 0.6951
- Chinese
- 🔍 Analysis: recording_1770550282937.wav → Real: 0.4838, Fake: 0.5162
- English
- 🔍 Analysis: recording_1770550371556.wav → Real: 0.2579, Fake: 0.7421

// dataset 
- 🔍 Analysis: convert_LA_E_1027501_1770563128.wav → Real: 0.3330, Fake: 0.6670
- 🔍 Analysis: convert_LA_E_1000273_1770563099.wav → Real: 0.0500, Fake: 0.9500
- 🔍 Analysis: convert_LA_E_1000791_1770562982.wav → Real: 0.0000, Fake: 1.0000
- 🔍 Analysis: convert_LA_E_1000147_1770562955.wav → Real: 0.3593, Fake: 0.6407

#### next step : 
1. consider :maybe : real % around 30% , or 10 %
- most real audio around 30% , and no fake audio large 10 %
- maybe 3 option , 25 up show real , lower 10 show fake , middle maybe ??? 
3.  upload page more that one file , quick start ? 
4.  creare own voice cloneing ? 

## 8/2/2026
#### change the real %
- 統一解決方案 - 創建 static/ai-result.js 共用模組
- const REAL_THRESHOLD = 0.10;  // 改成10%
- change it to 10 %, as there are no fake file higher that 10 %

// Replace the AI result section with:

    elements.result.innerHTML = result.prob_real > 0.1 ? 
        '<span class="green">✅ REAL</span>' : 
        '<span class="red">❌ FAKE</span>';
    elements.confidence.innerHTML = '';  // Hide percentages

// Update HTML templates 

    <script src="/static/ai-result.js"></script>

#### upload page to quick start 
- can upload 1 -10 file , just one button 
#### demo  video 

## 16/2/2026 /realtime_continuous
<img width="405" height="154" alt="image" src="https://github.com/user-attachments/assets/00602302-bf2d-4bf0-b152-4c865d6882db" />

- 每4秒自動切片分析，顯示Model原始輸出(Real: 0.7530, Fake: 0.2470)。
- 可調threshold(10%/30%/50%)，音量偵測(RMS)，連續滾動顯示50行結果


## 真實用戶模擬細節: 
- Upload頁面測試 ≡ 用戶拖曳FLAC → 點Analyze按鈕
- Record頁面測試 ≡ 用戶錄音5s → 自動轉16kHz → Analyze  
- Realtime頁面 ≡ 每4s自動觸發 (已實作前端測試)

## 21/2/2026 /evaluation
- 1. /preprocess : 從標籤檔提取 + 隨機選10,000檔案腳本
- jupyter:(venv) carmen@hkmu-1080ti:~/asvspoof$ python prepare_eval_dataset.py => eval_10000_dataset.csv
  
      (venv) carmen@hkmu-1080ti:~/asvspoof$ ls -la eval_10000_dataset.csv
        -rw-r--r-- 1 carmen carmen 230015 Feb 25 01:02 eval_10000_dataset.csv
      head eval_10000_dataset.csv
        filename,label
        LA_E_2714189.flac,real
        LA_E_8624167.flac,real
        LA_E_6677864.flac,real
        LA_E_1357107.flac,real
        LA_E_7531986.flac,real
        LA_E_9050668.flac,real
        LA_E_8353060.flac,real
        LA_E_2368562.flac,real
        LA_E_6619416.flac,real

## 22/2/2026 report--draft 
- ASVspoof 2019 (10,000 FLAC) → 自動上傳你的各HTML頁面 → 記錄真實時間 → 收集Model輸出 → 生成統計報告 
  - /eval_platform.py

        BASE_URL = "http://localhost:5001"  # 你的 Flask 服務
        FLAC_DIR = Path("/home/carmen/asvspoof/datasets/LA/ASVspoof2019_LA_eval/flac")
        DATASET_CSV = "/home/carmen/asvspoof/preprocess/eval_20_dataset.csv" 
        RESULTS_CSV = "/home/carmen/asvspoof/results/platform_eval_results_20.csv"
        SUMMARY_JSON = "/home/carmen/asvspoof/results/platform_eval_summary_20.json"


- python eval_platform.py
  
        📁 使用 FLAC 路徑: /home/carmen/asvspoof/datasets/LA/ASVspoof2019_LA_eval/flac
        📁 使用 CSV 資料集: /home/carmen/asvspoof/preprocess/eval_20_dataset.csv
        🚀 預設測試 20 個檔案
        🔥 開始平台真實用戶評估 (upload + analyze only)...
        ✅ 載入CSV資料集: 20 個有效檔案
           真實: 10
           假: 10
        📊 準備測試 20 個檔案
        Upload/Analyze: 100%|████████████████████████████████████████████████████████████████| 20/20 [00:00<00:00, 10434.89it/s]
        收集結果: 100%|█████████████████████████████████████████████████████████████████████| 20/20 [00:00<00:00, 152797.96it/s]
        ✅ 完成！成功測試 20 個檔案
        
        ==========================================================================================
        🎯 PLATFORM COMPREHENSIVE EVALUATION REPORT
        ==========================================================================================
        📁 總測試檔案: 20
        🎵 真實:  10 | 假:  10
        
        ⏱️  TIME ANALYSIS (upload + analyze only)
        ------------------------------------------------------------
        💾 總運行時間:       215.5s
        ⚡ 最快單檔:         5.488s
        🐌 最慢單檔:        16.466s
        📈 平均單檔時間:    10.775s
        🎯 P95時間:         14.728s
        ⬆️  上傳平均時間:    0.029s
        🔍 分析平均時間:    10.746s
        
        🤖 MODEL PERFORMANCE (Real Probability Score)
        ------------------------------------------------------------
        ✅ 真實聲音 (Real Score):
           最低:  1.0000 | 最高:  1.0000 | 平均:  1.0000
        ❌ 虛假聲音 (Real Score):
           最低:  0.0000 | 最高:  0.8765 | 平均:  0.0903
        
        📊 STANDARD DETECTION METRICS
        ------------------------------------------------------------
        🎯 EER (等錯誤率):     0.0000
        📈 AUC (ROC曲線):      0.0000
        ⚖️  EER最佳閾值:      inf
        
        💾 報告已保存: /home/carmen/asvspoof/results/platform_eval_results_20.csv | /home/carmen/asvspoof/results/platform_eval_summary_20.json

- /home/carmen/asvspoof/results/platform_eval_summary_20.json
  
        {
          "dataset": {
            "total": 20,
            "real": 10,
            "fake": 10
          },
          "timing": {
            "program_total": 58.02328409199981,
            "total_runtime": 220.86291885375977,
            "min_time": 5.494817495346069,
            "max_time": 16.77876114845276,
            "avg_time": 11.043145942687989,
            "p95_time": 15.557893061637879,
            "upload_avg": 0.025340175628662108,
            "analyze_avg": 11.017805767059325,
            "analyze_min": 5.4637041091918945,
            "analyze_max": 16.751309156417847,
            "files_count": 20
          },
          "real_scores": {
            "count": 10,
            "min": 0.9999924898147583,
            "max": 0.9999998807907104,
            "avg": 0.9999985694885254
          },
          "fake_scores": {
            "count": 10,
            "min": 3.5012819807889173e-06,
            "max": 0.8765178322792053,
            "avg": 0.09026941099657507
          },
          "metrics": {
            "eer": 0.0,
            "auc": 0.0,
            "eer_threshold": Infinity
          }
## 23/2/2026 real-time
-test user: line 
- with ThreadPoolExecutor(max_workers=20) as executor:  # 100→20

- 🚨 100線程超載！Flask服務崩潰
  - ❌ 測試失敗 LA_E_6422459.flac: HTTPConnectionPool(host='localhost', port=5001): Read timed out. (read timeout=6)

- cpu usage :　ps aux --sort=-%cpu | head -10

      USER         PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
      carmen      1834 80.5 71.3 20670896 11853808 pts/0 Sl+ 17:53 107:34 python app.py

- run 128 analyze in 20:00:01 → 20:11:29 = 11.28s  => 128 ÷ 11.48 = 11/fps and 89%cpu

### CPU analysis : 
- start the  app.y only need less then 2% CPU , and 9.9% me,pry
- watch -n 1 'ps aux --sort=-%cpu | head -n 10'

      USER         PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
      carmen      1433  2.0  9.9 2651760 1654912 pts/0 Sl+  14:23   0:06 python /home/carmen/asvspoof/program/app.py

- 1000 data :
  - 50 line : can't  
    - carmen      1433 71.3 45.9 13661776 7623840 pts/0 Sl+ 14:23   5:44 python /home/carmen/asvspoof/program/app.py
    - ❌ 測試失敗 LA_E_1686710.flac: HTTPConnectionPool(host='localhost', port=5001): Max retries exceeded with url:
  - error : put too many , soloution each Thread add 10 sec for analyze. 
- batch_size : 執行完整評估 - 批次上傳，每批之間等待10秒
  - 20 : stil overload server
    - but CPU:
      - 23.9 18.0 5924592 3000608 pts/0 Sl+  14:33   2:24 python /home/carmen/asvspoof/program/app.py
      - 130 27.5 7016384 4572208 pts/0 Sl+  14:33  19:08 python /home/carmen/asvspoof/program/app.py
  - 10s stil too short , may take the max file time (最慢單檔: 16.466s) : wait 20s 

- 動態並發：analyze response回來就放新檔案，總數永遠≤10
  - ❌ 20 : when start CPU 180 %CPU
  - 10 : evaluator.run_evaluation(max_files=1000, max_concurrent=10)
    - first thread only 80 , when doen 185 file => 293 , add 1 cpu when done file
    - carmen      8812  293 23.6 5662400 3924352 pts/0 Sl+  14:53  48:26 python /home/carmen/asvspoof/program/app.py

- 每完成1個檔案等幾秒冷卻，避免CPU持續累積
  - cooldown_sec=2 => ✅ 檔案完成，等待 2s 冷卻..

- Ctrl+C 中斷後，下次自動從上次結束處繼續
  - CHECKPOINT_CSV = "/home/carmen/asvspoof/results/platform_eval_checkpoint.csv"
  - 支援 4 種中斷，全部能續傳
    - 1. ✅ Ctrl+C（用戶手動）→ 斷點保存
    - 2. ✅ OOM（記憶體不足）→ 斷點已保存
    - 3. ✅ Timeout（超時崩潰）→ 斷點已保存  
    - 4. ✅ 其他異常（硬崩潰）→ 斷點已保存
  - 
