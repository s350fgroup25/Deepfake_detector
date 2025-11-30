# update 1/12/2025 
### Structure:
~/asvspoof/program/
├── app.py (已更新)
├── templates/
│   ├── home.html (新增首頁)
│   ├── index.html (上載頁面)
│   └── record.html (錄音頁面)
└── static/
    ├── actions.js (上載JS)
    └── record_actions.js (錄音JS)

# update 15/11/2025 
### Structure:
        ~/asvspoof/
        ├── program/
        │   └── app.py                # Your Flask app backend
        ├── S_audio/                  # Folder to save/upload audio files
        ├── www/
        │   ├── static/
        │   │   ├── actions.js        # JavaScript frontend logic
        │   │   └── images/
        │   │       └── audio-bg.jpg  # Static image assets
        │   └── templates/
        │       └── index.html        # HTML template for your web UI

### Usage Instructions
- 1.Connect via SSH:
  
        ssh carmen@192.168.1.149
- 2.Activate Python virtual environment:

        source venv/bin/activate
- 3.Run Flask backend app
  - 3a. For basic app:

        python ~/asvspoof/program/app_plain.py
  - 3b.For advanced or structured app:

        python ~/asvspoof/program/app.py

- 4.Access frontend web UI:

        App Plain: http://127.0.0.1:5001
        Other App: http://127.0.0.1:5000

- 5.Navigate to dataset folder for manual testing and exploration:

        cd ~/asvspoof/datasets/LA/ASVspoof2019_LA_eval/flac
        nano ~/asvspoof/datasets/LA/ASVspoof2019_LA_cm_protocols/ASVspoof2019.LA.cm.eval.trl.txt
- 6.Run single audio file test with your model script:

        python ~/asvspoof/program/eval_TF.py LA_E_5849185.flac
        python ~/asvspoof/program/eval_TF_.py LA_E_5849185.flac

## 重點摘要
- 你可以用 Flask 搭配基本的 HTML + JavaScript 完成這個 DEM 網站，
- 前後端都容易部署和修改，而且能直接在你的 Linux 主機上執行 Python/PyTorch 分析。
- Flask 做後端 API，上載/分析音檔
- 靜態 HTML+JS 做互動前端
- 音檔上限/格式直接在 JS 與 Flask server 雙重驗證
- test_sample.py 用 subprocess 呼叫分析
- 結果即時顯示 Real/Fake
- 所有成功檔案儲存 /home/carmen/asvspoof/S_audio
- 按圖示步驟即可完成 DEM 網站

## 1. 安裝必要工具和環境
- 確認主機已安裝 Python 與 Flask：
pip install flask

- 要處理多種音訊格式建議安裝：
pip install soundfile pydub
sudo apt install ffmpeg

## 2. 設計 Flask 後端結構
目錄整理
- 預設項目根目錄 /home/carmen/asvspoof/program
- 新增上載資料夾 /home/carmen/asvspoof/S_audio
- 放 model 檔案於 /home/carmen/asvspoof/program/wavlm-epoch50.safetensors
- test_sample.py 必須能像子程序被呼叫（用 subprocess）
- create :app.py
## 3. 設計主頁面HTML (templates/index.html)
## 4. 加上 JavaScript 前端邏輯 (static/actions.js)
- 控制上載流程、提交按鈕、顯示訊息、progress 條和結果。
- 主要是用 AJAX（fetch）呼叫 Flask API。

## 5. 測試 + Debug 指引
- 先啟動 Flask: python app.py
- 用 Chrome/Edge 開 http://localhost:5000
- 上載檔案，測試流程
- 若 test_sample.py 不支援傳入路徑，要在 subprocess 裡改引數
- test_sample.py 輸出結果應明確，例如 print 'True'/'False'。

## 6. 高級功能與注意事項
- 若安全性要求高，建議額外處理用戶權限、路徑檢查。
- 可以用 Gunicorn, Nginx (或 Docker) 做正式部署。
- 若未來想進階前端效果，可導入 Vue/React，但 demo 可用原生 JS 完成大部分互動。
- 如果 test_sample.py 分析過程長，可以考慮用 WebSocket 顯示進度條/即時回覆。

## covert flac to wav 
        mkdir -p /home/carmen/asvspoof/My_audio
        cd /home/carmen/asvspoof/datasets/LA/ASVspoof2019_LA_eval/flac
        for file in *.flac; do
            ffmpeg -i "$file" -acodec pcm_s16le "/home/carmen/asvspoof/My_audio/${file%.flac}.wav"
        done


## 修改 python File使其內容只會return T/F
        /home/carmen/asvspoof/My_audio/LA_E_1345816.wav
        LA_E_1115474.wav  LA_E_1235441.wav  LA_E_1356990.wav  LA_E_1479486.wav
        LA_E_1115487.wav  LA_E_1235532.wav  LA_E_1357041.wav  LA_E_1479551.wav


