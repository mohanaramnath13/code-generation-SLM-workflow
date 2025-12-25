## Code to evaluate deepseek-coder-1.3b-instruct

```python
python -m lcb_runner.runner.main     --model deepseek-ai/deepseek-coder-1.3b-instruct     --scenario codegeneration     --codegen_n 10     --evaluate     --timeout 30     --release_version main
```

results
```
        "pass@1": 0.09949999999999999,
        "pass@5": 0.14621031746031746,
        "pass@10": 0.155,
```
