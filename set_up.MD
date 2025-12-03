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

