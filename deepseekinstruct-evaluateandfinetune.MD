```python
python -m lcb_runner.runner.main     --model deepseek-ai/deepseek-coder-1.3b-instruct     --local_model_path /home/kbs/LCB_setup2/lcb_fork1/Qwen2.5-Coder-7B-Instruct-rStar-I2O-2000/     --scenario codegeneration     --codegen_n 10     --evaluate     --timeout 30     --release_version main     --custom_output_save_name qwen2.5_coder_7b_finetuned_pass10_full_run     --stop "<|im_end|>,<|endoftext|>,user,assistant"
```
