## 1. Installed WSL
## 2. ssh-ed into the server

## 3. Directory structure: 

~/LCB_setup1/
   
   ├── env_1/             ← conda environment (Python + libs only)
   
   └── livecodebench/     ← Git repo (code only)

## 4. Installing miniconda
cd ~
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh

bash Miniconda3-latest-Linux-x86_64.sh

## 5. Installed miniconda version 25.9.1

## 6. Creating conda environment and activating it
conda create -p ~/LCB_setup1/env_1 python=3.10 -y
conda activate ~/LCB_setup1/env_1

removal commands if necessary: 
conda deactivate
rm -rf ~/LCB_setup1/env_1

## 7. Installations:

###    a) Set up tools:
python -m pip install --upgrade pip setuptools wheel

###    b) torch
python -m pip install --upgrade "torch" --index-url https://download.pytorch.org/whl/cu121
python -m pip install --upgrade torchvision --index-url https://download.pytorch.org/whl/cu121

verifying: 
python - <<'PY'
import torch
print("torch", torch.__version__, "cuda_available:", torch.cuda.is_available())
print("CUDA devices:", torch.cuda.device_count())
PY

torch 2.5.1+cu121 cuda_available: True
CUDA devices: 2

###   c) vLLM
python -m pip install "vllm==0.10.2"

## 8. Hugging Face + datasets + transformers + support libs (pin datasets to avoid dataset-script removal issue)
python -m pip install "transformers[sentencepiece]" huggingface_hub accelerate
