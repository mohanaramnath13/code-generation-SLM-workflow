parser.py: default timeout : 6 seconds

10 seconds timeout still yielded less result : 0.42474999999999996

so I'm switching it to 30 seconds

```python
python -m lcb_runner.runner.main \
    --model Qwen/Qwen2.5-Coder-7B-Instruct \
    --scenario codegeneration \
    --continue_existing_with_eval \
    --evaluate \
    --timeout 30 \
    --release_version main \
    --custom_output_save_name qwen2.5_coder_7b_pass5_full_run
```
30 seconds timeout yielded this result : 0.4275

So we just see evaluation now, scope for improvement: 
1. using a different/larger model
2. Finetuning this model
