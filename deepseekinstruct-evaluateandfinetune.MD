## Code to evaluate deepseek-coder-1.3b-instruct

```python
python -m lcb_runner.runner.main     --model deepseek-ai/deepseek-coder-1.3b-instruct     --scenario codegeneration     --codegen_n 10     --evaluate     --timeout 30     --release_version main
```

results
```
        "pass@1": 0.09949999999999999,
        "pass@5": 0.14621031746031746,
        "pass@10": 0.155,

--- Failure Analysis by Platform and Difficulty (LOW TIMEOUT DATA) ---
================================================================================
| platform   | difficulty   |   Total_Problems |   Total_Failures |   Pass@10_Rate |
|:-----------|:-------------|-----------------:|-----------------:|---------------:|
| leetcode   | medium       |               90 |               84 |      0.0666667 |
| atcoder    | medium       |               76 |               74 |      0.0263158 |
| atcoder    | hard         |               58 |               58 |      0         |
| atcoder    | easy         |               76 |               43 |      0.434211  |
| leetcode   | easy         |               62 |               41 |      0.33871   |
| leetcode   | hard         |               29 |               29 |      0         |
| codeforces | easy         |                4 |                4 |      0         |
| codeforces | hard         |                3 |                3 |      0         |
| codeforces | medium       |                2 |                2 |      0         |
```
