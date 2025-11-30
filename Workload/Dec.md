## 1/12/2025
#### 1. mission : 
- 右下角顯示計時器，以 hh:mm:ss 格式顯示運行時間，每秒更新一次。
- 簡化的頁面佈局，只有兩個按鈕：選擇檔案和上傳並分析。
- 客戶端 JS 處理文件選擇、驗證、帶有進度條的上傳和分析顯示。
- 在應用程式啟動時全域載入一次模型（在全域變數中）
- 運行應用程序，監聽所有接口，端口 5001，關閉調試模式。
- 部署為生產 Web Server (http://192.168.1.149:5001/)

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


#### 3. record
<img width="929" height="720" alt="image" src="https://github.com/user-attachments/assets/e8ffc887-b7b9-4a66-b400-e5d9314118d3" />
<img width="556" height="599" alt="image" src="https://github.com/user-attachments/assets/e38057c2-0fe4-4f74-a532-261b04e85926" />
