## 1/11/2025
### fix bug : 
- requirements.txt
  -  ERROR: Could not find a version that satisfies the requirement nvidia-cudnn-cu11==9.1.0.70
  - ARM（例如 Raspberry Pi），無法使用官方的 NVIDIA cuDNN pip wheel。
    
  - Disabling PyTorch because PyTorch >= 2.1 is required but found 2.0.1
  - ImportError:Wav2Vec2Model requires the PyTorch library but it was not found in your environment
  - 官方的 PyTorch 最新版本（2.1+）尚未推出 ARM（Raspberry Pi）版本。

### fix bug 
- check what it the PyTorch version i used now !!!! 
- 1. 請使用 PyTorch 2.0.1 並調整您的程式碼：
  - 如果相容，請嘗試使用 PyTorch 2.0.1 的功能。
  - 有些轉換器或函式庫可能需要特定的版本，因此請嘗試降級轉換器或修改您的程式碼以使其與 PyTorch 2.0.1 相容。

- 2. 安裝 PyTorch 2.1+ 的變通方法：
  - 尋找第三方社群建置版本（例如 piwheels 或 GitHub 倉庫），這些版本可能包含較新的 PyTorch 版本。
  - 雖然可以從原始碼編譯，但這在樹莓派上非常耗費資源和時間。

- 3. 從原始碼建立 PyTorch
  -  Raspberry Pi 沒有 CMake 版本 3.18 或更高版本，這是從原始程式碼建置 PyTorch 所必需的
  - 在樹莓派上建造 PyTorch 需要很長時間——通常需要幾個小時甚至一天以上
### bug : 
- _bz2
  - 缺少的系統庫（_bz2）阻止了Python內建的bz2模組加載失敗
  
        ModuleNotFoundError: No module named '_bz2'
        ModuleNotFoundError: Could not import module 'Trainer'. Are this object's requirements defined correctly?
  - 由於該transformers套件torchvision在初始化期間依賴 bz2，這會引發一系列次要錯誤，導致 Transformers 的導入（例如Trainer）失敗
  - 修復修復缺少的_bz2模組


        sudo apt install libbz2-dev
        cd /usr/src/python3.11   # or the directory where your Python source is located
        sudo apt install python3-dev
        sudo apt install build-essential libssl-dev zlib1g-dev libncurses5-dev \
          libffi-dev libsqlite3-dev libreadline-dev liblzma-dev libbz2-dev
        sudo apt install libbz2-1.0
        sudo reboot
      python3 -c "import bz2; print('bz2 ok')"
  - 修復 bz2 後，請測試您的環境
  
          python3 -c "from transformers import Trainer; print('Transformers ok')"
  -重新運行
  
        python3 test_all.py

- bulid the pytorch :
  - https://github.com/pytorch/pytorch/tree/main
  
        /home/carmen/asvspoof/pytorch/torch/csrc/jit/python/pybind_utils.h:1076:19: note: remove ‘std::mov
        [2310/6484] Building CXX object caffe2/torch/CMa.../torch_python.dir/csrc/distributed/rpc/init.cpp.

- 安裝所需的 bzip2 開發包：
  
    sudo apt update
    sudo apt install libbz2-dev
- 重新建置或重新安裝 Python，使其能夠編譯該_bz2模組 (I WANT TO USE 3.12)
  
    cd /path/to/python-source
    make clean
    ./configure --enable-optimizations
    make -j4
    sudo make altinstall

- 重新編譯後進行驗證
  
    python3 -c "import bz2; print('bz2 module loaded')"

## fix bug ：
- can't just choose some .flac file 
- 請確認.flac資料目錄中的所有檔案均完整且可存取。
- 如果某些檔案損壞或遺失，您可能需要修復或刪除這些檔案。


## 2/11/2025
- delete all & try to re-bulid all
- **fix _bz2 (done)**
  - delete all python packet
  - download python 3.12.3  
- **fix pytorch (done)**
  - pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cpu
- **fix Transformers (done)**
  - pip install transformers safetensors numpy soundfile

## 3/11/2025
- **Run test_all.py** 
  - ImportError: Using the `Trainer` with `PyTorch` requires `accelerate>=0.26.0`: Please run `pip install transformers[torch]` or `pip install 'accelerate>=0.26.0'`
model.safetensors: 100%|██████████████████████████████████████████████████████████| 378M/378M [00:48<00:00, 7.74MB/s]
  - pip install --upgrade 'transformers[torch]' accelerate>=0.26.0
  - pip show accelerate
  <img width="948" height="253" alt="image" src="https://github.com/user-attachments/assets/8931195f-4a56-4dbb-b614-2a5f2bba3efc" />
- can run , no error , but no response , maybe need lots of time

  - /home/carmen/venv/lib/python3.12/site-packages/torch/utils/data/dataloader.py:668: UserWarning: 'pin_memory' argument is set as true but no accelerator is found, then device pinned memory won't be used.warnings.warn(warn_msg)
- Lower the num_workers parameter in your DataLoader to 2 or 4 (or 0 to disable multiprocessing):
- In your Python script or wherever you create the DataLoader, modify it like this:
  - DataLoader(dataset, batch_size=..., shuffle=..., num_workers=2, pin_memory=False)
  <img width="934" height="37" alt="image" src="https://github.com/user-attachments/assets/b57fc301-6746-45f9-b0a6-03b7f060821a" />
    - In Jupyterlab : 9%|█████▋| 49/557 [04:35<48:05,  5.68s/it]
    - In raspberry :1%|▋| 5/557 [26:49<54:43:45, 356.93s/it] (start to look for result)
      - around 5 min per 1 sample
      - need more and more time as, start was ~32h but after 5 sample it need ~54h

### Run test_10.py
  - if num_samples > 10:
     - indices = random.sample(range(num_samples), 10)
  <img width="973" height="717" alt="image" src="https://github.com/user-attachments/assets/51c7d172-9059-49a4-b77d-8bf865d7ec7e" />
  
- In Jupyterlab :
  - Total evaluation time (s): 0.752 per 10 sample
- In raspberry pi 5 :
  - Total evaluation time (s): 25.322 per 10 sample
  - avg :2.5s per 1 audio , (not correct , as total ~3s)

## 4/11/2025
### Run eval-sentence.py
- chage the max_eval_samples don't work ( Stil = ... /557)

### Jupyterlab 
- sample = 1 (test_sample_skip.py)
  
        Model loaded successfully in 1.423 seconds.
        Fetching 1 files: 100%|███████| 1/1 [00:00<00:00, 15363.75it/s]
        100%|███████████████████████████| 1/1 [00:00<00:00, 921.22it/s]
        Skipping EER calculation due to single sample evaluation.
        Positive class scores: [1.6212082e-11]
        Labels: [0]
  
- sample = 10 (test_sample.py )

      Model loaded successfully in 1.375 seconds.
      Fetching 1 files: 100%|███████| 1/1 [00:00<00:00, 11275.01it/s]
      100%|██████████████████████████| 10/10 [00:00<00:00, 29.42it/s]
      nontarget_scores[nontarget_position] is 7 9.570489e-11
      target_scores[target_position] is 0 1.0
      threshold  1.0
      Equal Error Rate (EER): 0.0

- sample = 100 (test_sample.py )
  
      Model loaded successfully in 1.456 seconds.
      Fetching 1 files: 100%|███████████████████████████████████████████████| 1/1 [00:00<00:00, 15534.46it/s]
      100%|████████████████████████████████████████████████████████████████| 100/100 [00:03<00:00, 31.05it/s]
      nontarget_scores[nontarget_position] is 85 0.9999995
      target_scores[target_position] is 0 1.0
      threshold  1.0
      Equal Error Rate (EER): 0.0

- sample = all (71237) (test_sample.py )

      Model loaded successfully in 1.407 seconds.
      100%|████████████████████████████████████████████████████████████| 71237/71237 [38:15<00:00, 31.03it/s]
      nontarget_scores[nontarget_position] is 62838 0.9999998
      target_scores[target_position] is 120 0.9999999
      threshold  0.9999999
      Equal Error Rate (EER): 0.016315431679129844

### Raspberry Pi 5
- sample = 1 (test_sample_skip.py)

      Model loaded successfully in 9.393 seconds.
      100%|██████████████████████| 1/1 [00:00<00:00, 1208.38it/s]
      Skipping EER calculation due to single sample evaluation.
      Positive class scores: [2.0084073e-11]
      Labels: [0]
- sample = 10 (test_sample.py)

      Model loaded successfully in 1.211 seconds.
      100%|█████████████████| 10/10 [00:16<00:00,  1.63s/it]
      nontarget_scores[nontarget_position] is 7 1.7440004e-10
      target_scores[target_position] is 0 1.0
      threshold  1.0
      Equal Error Rate (EER): 0.0
- sample = 100
  
      Model loaded successfully in 1.599 seconds.
      100%|████████████████████| 100/100 [03:40<00:00,  2.21s/it]
      nontarget_scores[nontarget_position] is 85 5.576394e-09
      target_scores[target_position] is 0 0.99183625
      threshold  0.99183625
      Equal Error Rate (EER): 0.0





### Run single_evaluate.py
  - Download wav :
    - Asvspoof2021 : https://www.asvspoof.org/index2021.html
      - ASVspoof2021_LA_eval.tar.gz :　https://zenodo.org/records/4837263
    - test_file = '2021LA1.wav'
    - https://zenodo.org/records/4837263
  - In Jupyterlab : no wav now , as can't sudo
  - In raspberry : Bugs

#### Install FFmpeg (raspberry)
- covert flac to wav
  
      sudo apt-get install ffmpeg
      cd /asvspoof/datasets/LA/ASVspoof2019_LA_eval/flac 
      ffmpeg -i LA_E_1000147.flac 
      /home/carmen/asvspoof/datasets/LA/ASVspoof2019_LA_eval/wav/LA_E_1000147.wav
  
#### Bug :
- 1. ImportError: TorchCodec is required for load_with_torchcodec.
    - pip install torchcodec
- 2. Errors were encountered while processing:apt-listchanges
  - E: Sub-process /usr/bin/dpkg returned an error code (1)
  - skip it 
- 3. AttributeError: module 'torchaudio' has no attribute 'set_audio_backend'
  - torchaudio.set_audio_backend("sox")  # or "soundfile"
  - SoX (Sound eXchange）? vs FFmpeg   ??? 
  - sudo apt-get install sox
  - sox input.flac output.wav 

    

## 10/11/2025
- draw UX (website & andriod app)
  ![WhatsApp Image 2025-11-10 at 9 31 20 PM](https://github.com/user-attachments/assets/7d6f957c-0580-4454-8e1b-b99036726e49)

## 11/11/2025
- Defalt Layout : 
<img width="719" height="496" alt="image" src="https://github.com/user-attachments/assets/4302f3ad-0080-4ab8-b37c-cb0df05765bc" />

- Function 1. checking file format
  - 1a. successfully
    - <img width="465" height="150" alt="image" src="https://github.com/user-attachments/assets/c8e1bc41-3bc9-434f-85db-4198bc05af2a" />
  - 1b (fail)
    - <img width="401" height="146" alt="image" src="https://github.com/user-attachments/assets/d6b83cff-0023-4a17-b517-d2d9923d953e" />

- Function 2. cancel
  - <img width="374" height="110" alt="image" src="https://github.com/user-attachments/assets/878ae85b-e56f-4321-82bc-f1522aacab98" />

- Function 3. submition 
  - <img width="450" height="351" alt="image" src="https://github.com/user-attachments/assets/e2961ea2-b91f-4203-86e8-dea3746bf14b" />
  - 3a successfully
  - 3b error
    - <img width="384" height="165" alt="image" src="https://github.com/user-attachments/assets/22b8b1b4-4afa-4575-9532-16fa4eee9613" />



