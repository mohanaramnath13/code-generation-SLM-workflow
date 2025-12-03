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

python -m pip install "datasets==2.14.2"

python -m pip install sentencepiece tokenizers safetensors jsonlines

conda install -y -c conda-forge "numpy<2" pyarrow=11.0.0 pandas
[to match versions]

Check compatibility:
python - <<'PY'
import sys, numpy as np, pyarrow as pa, pandas as pd
import torch, vllm, transformers, datasets
print("Python:", sys.version.splitlines()[0])
print("numpy:", np.__version__)
print("pyarrow:", pa.__version__)
print("pandas:", pd.__version__)
print("datasets:", datasets.__version__)
print("torch:", torch.__version__, "cuda:", torch.cuda.is_available(), "devices:", torch.cuda.device_count())
print("vllm:", vllm.__version__)
print("transformers:", transformers.__version__)
PY

output:
Python: 3.10.19 (main, Oct 21 2025, 16:43:05) [GCC 11.2.0] numpy: 1.26.4 pyarrow: 11.0.0 pandas: 2.3.3 datasets: 2.14.2 torch: 2.8.0+cu128 cuda: True devices: 2 vllm: 0.10.2 transformers: 4.57.3


