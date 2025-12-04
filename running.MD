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

## 4. That didn't work, maybe need to add temperature and try to save the outputs

python -m lcb_runner.runner.main \
  --model meta-llama/Meta-Llama-3-8B-Instruct \
  --scenario codegeneration \
  --n 20 \
  --codegen_n 5 \
  --temperature 0.7 \
  --top_p 0.95 \
  --debug \
  --custom_output_save_name pass5_gen

## 5.That just created the samples, need to evaluate

python -m lcb_runner.runner.main \
  --model meta-llama/Meta-Llama-3-8B-Instruct \
  --scenario codegeneration \
  --evaluate \
  --custom_output_file output/LLama3-8b-Ins/Scenario.codegeneration_20_0.7.jsonl \
  --debug
