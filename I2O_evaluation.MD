Evaluating the fine-tuned I2O model:
```python
python -m lcb_runner.runner.main \
    --model Qwen/Qwen2.5-Coder-7B-Instruct \
    --local_model_path /home/kbs/LCB_setup2/lcb_fork1/Qwen2.5-Coder-7B-base-rStar-I2O-2000/ \
    --scenario codegeneration \
    --codegen_n 10 \
    --evaluate \
    --timeout 30 \
    --release_version main \
    --custom_output_save_name qwen2.5_coder_7b_finetuned_pass10_full_run
```
