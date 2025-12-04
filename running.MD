## 1. Smaller model

python -m lcb_runner.runner.main \
    --model deepseek-ai/deepseek-coder-1.3b-instruct \
    --scenario codegeneration \
    --evaluate \
    --release_version release_v5

## 2. Larger model

python -m lcb_runner.runner.main --model meta-llama/Meta-Llama-3-8B-Instruct --scenario codegeneration --evaluate --debug --n 1

## 3. Larger model with pass@5

python -m lcb_runner.runner.main   --model meta-llama/Meta-Llama-3-8B-Instruct   --scenario codegeneration   --evaluate   --codegen_n 5   --debug   --n 1
