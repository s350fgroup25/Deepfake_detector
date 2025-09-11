## info 
Username : pi 
password : pii 
## commend :
To GUI : startx
To CMD : Ctrl + Alt + F2

## workload
### 01/09/2025 
- doenload raspberry pi image 
- install and reset the OS
- Debian GUI/Linux 12 raspberrypi tty1
- set auto login to CMD

### 11/9/2025
- jupterhub : http://210.3.248.195:5200/hub
- try upload and unzip file to server (program.zip)
- try to run model
- install python packet

#### step 1 
      # Create project directory in your home folder
      mkdir -p ~/asvspoof
      cd ~/asvspoof
      
      # Create all necessary directories
      mkdir -p logs
      mkdir -p datasets/models
      
      # Create virtual environment using built-in venv (no conda needed)
      python -m venv venv
      
      # Activate virtual environment (Linux/Mac)
      source venv/bin/activate

#### step 2 
      # 安裝核心套件
      pip install torch torchaudio torchvision --index-url https://download.pytorch.org/whl/cu118
      
      # 安裝transformers和其他必要套件
      pip install transformers datasets soundfile numpy scikit-learn safetensors
      
      # 安裝開發工具
      pip install jupyterlab matplotlib tqdm

#### step 3
      # 進入數據集目錄
      cd datasets/LA
      
      # 下載數據集（需要從官方申請，這裡假設你已經有數據）
      - https://www.kaggle.com/datasets/awsaf49/asvpoof-2019-dataset?resource=download-directory&select=LA
      - upload to ~/asvspoof-demo
      - unzip LA.zip -d datasets

#### step 4 (in root )
      創建 requirements.txt
        torch==2.0.1
        torchaudio==2.0.2
        transformers==4.30.2
        datasets==2.13.1
        soundfile==0.12.1
        numpy==1.24.3
        scikit-learn==1.2.2
        safetensors==0.3.1
        tqdm==4.65.0
      創建所有必要的Python文件
      - upload program.zip
      - 修改 all path -- /home/carmen/asvspoof/datasets

#### step 5 運行訓練
      # 確保在虛擬環境中
      source venv/bin/activate  # Linux/Mac
      # 運行訓練腳本
      cd ~/asvspoof/program
      python train-sentence.py
      
      # debug
      1. NameError: name 'np' is not defined
        - add "import numpy as np" to train-sentence.py
      2.ImportError: Using the `Trainer` with `PyTorch` requires `accelerate>=0.26.0`
        - pip install accelerate>=0.26.0
        - pip install transformers[torch]
        - pip install datasets soundfile
        - pip list | grep -E "accelerate|transformers|torch"
      3.
        - GPU not enought :　cretae train-sentence-low-memory.py
        - package error : can't python 3.12 ,
        - need sudo , missing some system package, e.g libffi
        - 已被刪除/棄用的符號: PyUnicode_FromUnicode
      ## python 3.12 have too much error  
          carmen@hkmu-1080ti:~$ python3.8 -m venv venv38
          source venv38/bin/activate
1.TypeError: TrainingArguments.__init__() got an unexpected keyword argument 'evaluation_strategy'
- 請將所有 替換為evaluation_strategy，eval_strategy然後重新執行腳本。這應該可以解決 問題TypeError
2. ConnectionResetError: [Errno 104] Connection reset by peer
- 警告GradScaler表明較新的語法需要初始化，而torch.amp.GradScaler("cuda")不是已棄用的torch.cuda.amp.GradScaler()
- 如果您打算使用 WavLM，請確保您的模型類別Model正確建置以使用 WavLM 架構。
- 使用transformers.WavLMFeatureExtractor或正確的 WavLM 處理器，而不是Wav2Vec2FeatureExtractor。
- from transformers import Wav2Vec2FeatureExtractor => 
from transformers import WavLMFeatureExtractor
processor = WavLMFeatureExtractor.from_pretrained('microsoft/wavlm-base')
#### step 6 運行評估
      # 運行評估腳本
      python eval-sentence.py

