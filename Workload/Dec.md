## 1/12/2025
#### Mission : 
- 右下角顯示計時器，以 hh:mm:ss 格式顯示運行時間，每秒更新一次。 ()
- 簡化的頁面佈局，只有兩個按鈕：選擇檔案和上傳並分析。
- 客戶端 JS 處理文件選擇、驗證、帶有進度條的上傳和分析顯示。
- 在應用程式啟動時全域載入一次模型（在全域變數中）
- 運行應用程序，監聽所有接口，端口 5001，關閉調試模式。
- 部署為生產 Web Server (http://192.168.1.149:5001/)

#### done :
1. timer  :　upload than cal the time ~ 2s
2. user-friendly - good looking - only only button
3. web server : ip:port (access in different device)

#### 2. Fix bug :
1. bug: only can 16kHz 
  <img width="563" height="835" alt="image" src="https://github.com/user-attachments/assets/ae158d4d-dea3-4a6a-b4d9-6b74ed5e0a97" />

  - 現在可與任何取樣率搭配使用（44.1kHz、48kHz 等）→ 自動重採樣至 16kHz！
  - 定時器測量精確的上傳+分析時間。
  - 確認按鈕結果會一直顯示，直到準備好處理下一個文件。
  - 不再出現 JSON 錯誤－僅支援原生 Python 布林值。
2.  answer wrong : 
  <img width="962" height="928" alt="image" src="https://github.com/user-attachments/assets/e62c7d47-2975-4d23-9d63-6748aa9edb87" />

- with other audio will not correct
- but correct specific dataset (asvspoof 2019)
<img width="911" height="576" alt="image" src="https://github.com/user-attachments/assets/6b8862e5-3c8e-4412-bf0c-61afe6c75c30" />


## 4/12/2025
#### Record
<img width="929" height="720" alt="image" src="https://github.com/user-attachments/assets/e8ffc887-b7b9-4a66-b400-e5d9314118d3" />
<img width="556" height="599" alt="image" src="https://github.com/user-attachments/assets/e38057c2-0fe4-4f74-a532-261b04e85926" />

#### 4. HTTPS：　
- 你的情況：RPi 作為 server，客戶端用 Windows/Mac 手機瀏覽器錄音 → 必須 HTTPS。
- 終極方案：Flask ad-hoc SSL + 防火牆 + 瀏覽器特殊處理

<img width="911" height="191" alt="image" src="https://github.com/user-attachments/assets/666222d6-c5a8-4f5b-8bc7-29bffa46aeff" />

<img width="735" height="643" alt="image" src="https://github.com/user-attachments/assets/1491ab2c-4e10-4980-8c1b-5ac3b4099d67" />

- still fail !!!

####  HTTP: (can't record)
        if __name__ == '__main__':
            app.run(host='0.0.0.0', port=5001, debug=False)  # 暫時去掉 ssl_context

####  HTTPS: (can't connect)
-  record_actions_odd.js
1. ssl_context='adhoc' (fail)

        pip install cryptography
        if __name__ == '__main__':
            app.run(host='0.0.0.0', port=5001, ssl_context='adhoc', debug=False)

2. openssl 自簽憑證：
   
        openssl req -x509 -newkey rsa:4096 -keyout key.pem -out cert.pem -days 365 -nodes -subj "/CN=192.168.1.149"
        修改 app.py 最後一行：
        app.run(host='0.0.0.0', port=5001, ssl_context=('cert.pem', 'key.pem'), debug=False)

#### 新增 HTTP 相容 record_actions.js
- 用 AudioContext 繞過 getUserMedia 的 HTTPS 限制，直接用瀏覽器內建音頻處理。
#### Chrome（旗標啟用）：
- 核心問題：navigator.mediaDevices 在 HTTP + 非 localhost 環境 = undefined。瀏覽器安全策略無法繞過！
1. chrome://flags/#unsafely-treat-insecure-origin-as-secure
2. 輸入：http://192.168.1.149:5001
3. Enabled → Relaunch
4. 訪問：http://192.168.1.149:5001/record


#### Done 
- record function 
- but answer not correct :
- record_odd.html + record_actions_odd.js
<img width="510" height="632" alt="image" src="https://github.com/user-attachments/assets/c318159d-12e5-441e-a36d-c2f9fb676eb5" />

## 6/12/2025
- found that the record function not really record the sound

### Fix 
- source venv/bin/activate
- python ~/asvspoof/program/app.py
- nano ~/asvspoof/www/templates/record.html
- nano ~/asvspoof/www/static/record_actions.js

#### ❌ Microphone error: Cannot read properties of undefined (reading 'getUserMedia')
<img width="719" height="339" alt="image" src="https://github.com/user-attachments/assets/b3b77527-9ba9-4799-8cb4-9388a799d687" />
Option A – Chrome flag (stay on HTTP, simplest)
- chrome://flags/#unsafely-treat-insecure-origin-as-secure​
- http://192.168.1.149:5001
<img width="823" height="205" alt="image" src="https://github.com/user-attachments/assets/af9b67e7-39fe-45ce-9cec-fed22bd12aee" />

- if can't record audio pls check you microphone
- https://vocalremover.org/zh/voice-recorder
<img width="675" height="121" alt="image" src="https://github.com/user-attachments/assets/56ba6450-af3a-4186-b9ac-d2e4f937c406" />

#### done 
- can truly record the voice and play and downlod

## 7/12/2025
- not problem of record part
  - can record / play / download
#### Try upload the record file and analze
- can upload but result not correct
<img width="575" height="581" alt="image" src="https://github.com/user-attachments/assets/d02851ac-11cd-4c7b-b612-d1b9b45b8023" />

#### problem :
- ❌ Upload failed: Invalid file type (.webm)
  - using FFmpeg 轉檔
    <img width="1027" height="273" alt="image" src="https://github.com/user-attachments/assets/1192454f-dee5-429d-bd09-b2c5ec61e60c" />

    <img width="1071" height="416" alt="image" src="https://github.com/user-attachments/assets/3629b1a9-546b-4990-89e4-e89a72c15efe" />

- Hz wrong : 
  - Problem in convert file type (wav) or 48kHz to 16kHz 

#### Note 
- 結果判定是假的，可能原因包含：
  - 錄音格式或音質有問題（頻率、通道數）
  - 麥克風輸入有干擾或過小聲（導致模型判斷為假）
  - 轉檔過程中音頻品質降低（雖然轉了16kHz單通道）
 
- 測試使用外部 WAV 音檔 (用已知好聲音的 WAV 音檔做推論，看模型是否正常判斷真／假)
- Now only flac 100% correct
- Window record voice :file m4a
- supporting file type : as normal as commom

## 17/12/2025
1. add home button to each page => user friendly
- <img width="121" height="41" alt="image" src="https://github.com/user-attachments/assets/bc1b661c-be61-4f5a-bcf2-0db52135fb4e" />

2. Fix the rocord page
  - change the record type form .webm to wav 
  - record_actions.js：改成直接錄 WAV（16kHz PCM）
    - 改成錄 raw PCM，自己封裝成 16kHz/mono/16bit WAV
    - as 原生 MediaRecorder 幾乎都不支援 audio/wav 直接輸出，只支援 audio/webm / audio/ogg 等壓縮格式
    <img width="543" height="490" alt="image" src="https://github.com/user-attachments/assets/f4fb7b82-fa93-4174-9a38-d08426d0f973" />
3. result still fake
  - <img width="516" height="71" alt="image" src="https://github.com/user-attachments/assets/9f287a18-2fda-4200-8195-52db27cbbad5" />
  - try find reason , now only the sample show ture
  - test other sample form other dataset 


## 19/12/2025
- pip install ffmpeg-python
- nano ~/asvspoof/www/templates/convert.html
- nano ~/asvspoof/www/static/convert.js
- create a new page convert.html that support common audio type
  - convert flac , m4a , mp3 ,webm to wav and 16kHz
  - 後端 /convert 用 ffmpeg 統一轉成 16kHz mono WAV

## Home page : 
<img width="913" height="856" alt="image" src="https://github.com/user-attachments/assets/6bc2388f-f434-4bd4-86ef-ba235869d008" />

## convert page
<img width="512" height="577" alt="image" src="https://github.com/user-attachments/assets/7ebf1e0a-4c09-4de7-842b-878412c0e565" />

- need to click convert button then have next function

<img width="488" height="932" alt="image" src="https://github.com/user-attachments/assets/51e17f94-aa1d-43cc-9552-f8f8bd6bdc43" />

- also can listen some not driectly audio type , e.g flac 
- driectly submit to model , pls consider delete the upload page , as function deplicate

## 20/12/2025
- nano ~/asvspoof/program/app.py
- nano ~/asvspoof/www/templates/fake.html
- nano ~/asvspoof/www/static/fake.js

- To test the model credibility :
  - create a fake audio using Text to speech method
  - pip install gTTS
  - clearly a obot , but show true !!!!!!!!!!!
- <img width="516" height="671" alt="image" src="https://github.com/user-attachments/assets/b0cbf987-9c1f-46d6-9620-3ed59234685b" />

- sometime fake 
<img width="469" height="629" alt="image" src="https://github.com/user-attachments/assets/c25a92e1-5c79-49b8-90ff-f238a974ed92" />

## 21/12/2025
- To test the model credibility :
  - put all covent file , record file , also fake file and use the single_evlauta.py
  - create a local file /testSet/record , /testSet/fake ,/testSet/covent
  - each create 3 sample : create wav file 
  - used the above sample to single_evlauta.py
  - cd asvspoof/S_audio
  - mv fake_1766340976.wav /home/carmen/asvspoof/testSet/fake/fake3.wav
  - ls /home/carmen/asvspoof/testSet
- result (orgigin , result  , single)
    1. /testSet/record ( T, F , ): hello, ( T, F , ) : goodmoring, ( T, F , ) : empty
    2. /testSet/covent ( T , F , ) - convert1 from .wemb , ( T , F , )- convert2 from .m4a ,  ( F , F , ): flac F , (T , T ) : flac T
    3. /testSet/fake ( F, T , ): i am fine , ( F, F , ): garbled char , ( F, T , ): <script>alert(1);</script>

- found that /convert and /fake no auto delete function
- No XSS injectiion 
