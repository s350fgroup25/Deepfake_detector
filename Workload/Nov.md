## 1/11/2025

- test_all.py
  - Output all print statements to test_output.txt:
      - Uses Python’s redirect_stdout to write printed output into a file.
- test_10.py
  - Randomly select 10 audio samples:
      - Modifies the dataset so only 10 randomly chosen samples are processed (if more than 10 samples available).
- Zip
  - packing all required file in zip
  - asvpoof2019.zip
    - /datasets/models/wavlm-epoch50/
    - /datasets/LA
        - /ASVspoof2019_LA_cm_protocols
        - /ASVspoof2019_LA_eval
        - /random_select_zip.py
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
  
- raspberry pi 5
  - import the test enviroment
  - try test.py
  - try single_evaluate 

### bug : 
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


## 方案一：使用預先編譯的 ARM64 架構 PyTorch
- 1. 將安裝僅 CPU版本，避免 CUDA 依賴項並節省空間，同時仍允許推理。
  - pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cpu
- 2. 安裝 Hugging Face 和 safetensors 庫：
  - pip install transformers safetensors numpy soundfile
- 3. run test_all.py

## 方案二：輕量級替代方案（如果 PyTorch 仍然失敗）
- 1.  將訓練好的.safetensors模型轉換為 ONNX 或 TorchScript
- 2.  在更強大的機器上使用ONNX Runtime運行推理
  - 在已安裝 PyTorch 的桌上型電腦上 (jupyterlab)
  
          import torch
          from model_sentence1 import Model
          from safetensors.torch import load_file
          
          model = Model(model_type='wavlm', device='cpu')
          state_dict = load_file('wavlm-epoch50.safetensors')
          model.load_state_dict(state_dict)
          model.eval()
          
          dummy_input = torch.randn(1, 16000)  # example 1s of audio
          torch.onnx.export(model, dummy_input, "wavlm_model.onnx", opset_version=12)
- 3. 將 ONNX 檔案複製到您的 Raspberry Pi
- 4. 安裝 ONNX 執行時間

      pip install onnxruntime onnx numpy soundfile
- 5. 修改test_all.py 檔案以使用

          import onnxruntime as ort
          # load model once
          ort_session = ort.InferenceSession("wavlm_model.onnx")
          
          # run inference instead of `trainer.predict(...)`
          outputs = ort_session.run(None, {"input": input_array.astype(np.float32)})
     
## 方案3 ：：僅運行推理部分
- 如果你已經實現了部分 PyTorch，但想要跳過整個 Hugging FaceTrainer邏輯
- 避免初始化TrainingArguments，並且Trainer保持最少的依賴項處於活動狀態
- 
            model.eval()
            with torch.no_grad():
                for i, batch in enumerate(eval_dataset):
                    inputs = batch["input_values"].to(device)
                    labels = batch["label"]
                    logits = model(inputs)
                    pred = torch.argmax(logits, dim=-1)
                    ...
## 概括
- 除非有非常特殊的需求，否則無需從原始碼建立 PyTorch 。
- 使用預先建置的 PyTorch wheels（pip 安裝）是啟用偵測的實用且簡單的方法。
- 修復 Python 系統相依性（例如 bz2），以避免使用過程中出現匯入錯誤。
- 對於像 Raspberry Pi 這樣資源受限的設備，僅支援 CPU 的 PyTorch wheels 或 ONNX 執行時間環境就足夠了。



## 專為內存有限的樹莓派量身定制的更簡便方法 
- 如果可能，請使用 PyTorch預先建置的輕量級版本（例如，適用於 ARM64 的 PyTorch 1.10 或 2.0）。
- 透過直接運行推理來避免使用繁重的 Transformer Trainer 和 TrainingArguments 機制。
- 使用最少的依賴項：torch、transformers 特徵提取器、safetensors 用於模型載入。
- 批次大小應保持較小（例如，1 或 2），以減少記憶體使用量。
- 由於樹莓派可能不支援 CUDA，請使用 CPU 裝置並停用 GPU 特定的環境變數。
- 簡化的推理範例（基於您的test_all.py核心邏輯 (re-write  code) -- test_less.py
- 
## 2/11/2025
- delete all
- try to re bulid all
