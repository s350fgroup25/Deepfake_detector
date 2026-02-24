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

## 21/2/2026 /evaluation
pre-process : 從標籤檔提取 + 隨機選10,000檔案腳本
jupyter:(venv) carmen@hkmu-1080ti:~/asvspoof$ python prepare_eval_dataset.py => eval_10000_dataset.csv
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

## 22/2/2026 
真實用戶模擬細節: 
Upload頁面測試 ≡ 用戶拖曳FLAC → 點Analyze按鈕
Record頁面測試 ≡ 用戶錄音5s → 自動轉16kHz → Analyze  
Realtime頁面 ≡ 每4s自動觸發 (已實作前端測試)

eval_10000_dataset.csv
PROTOCOL_FILE = "/home/carmen/asvspoof/datasets/LA/ASVspoof2019_LA_cm_protocols/ASVspoof2019.LA.cm.eval.trl.txt" 
FLAC_DIR = Path("/home/carmen/asvspoof/datasets/LA/ASVspoof2019_LA_eval/flac")


/eval_platform.py
=> ASVspoof 2019 (10,000 FLAC) → 自動上傳你的各HTML頁面 → 記錄真實時間 → 收集Model輸出 → 生成統計報告

Step 1: 載入資料
├── 掃描 /path/to/eval/flac/*.flac (10,000檔案)
├── 讀取 protocol.txt 標籤 (bonafide/spoof)
└── 準備測試清單

Step 2: 並行測試 (4線程同時)
├── Thread 1: Upload頁面測試 (10,000檔案)
│   ├── POST /upload → 獲取 server_filename
│   ├── POST /analyze → 獲取 Model輸出
│   └── 記錄: upload_time(0.2s) + analyze_time(1.2s) = total_time(1.4s)
│
├── Thread 2: Record頁面模擬 (1,000檔案)
│   ├── POST /convert → 模擬錄音轉16kHz
│   ├── POST /analyze → Model分析
│   └── 記錄: convert_time(0.8s) + analyze_time(1.2s)
│
├── Thread 3: 空閒/備用
└── Thread 4: 空閒/備用

Step 3: 即時統計
├── 真實聲音 Real分數: 最高0.99 / 最低0.72 / 平均0.89
├── 虛假聲音 Real分數: 最高0.12 / 最低0.001 / 平均0.03
└── 速度: 平均1.4s/檔案 (P95: 2.1s) → ✅ Realtime合格!

Step 4: 生成報告
├── platform_eval_results.csv (10,000行明細)
├── platform_eval_summary.json (統計摘要)
└── Console完整報告
