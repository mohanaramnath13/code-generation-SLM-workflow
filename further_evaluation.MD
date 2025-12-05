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

## Failure Analysis by Platform and Difficulty (30s TIMEOUT DATA)

| platform   | difficulty | Total_Problems | Total_Failures | Pass@10_Rate |
|------------|------------|----------------|----------------|--------------|
| atcoder    | medium     | 76             | 58             | 0.236842     |
| atcoder    | hard       | 58             | 57             | 0.0172414    |
| leetcode   | medium     | 90             | 29             | 0.677778     |
| leetcode   | hard       | 29             | 22             | 0.241379     |
| atcoder    | easy       | 76             | 16             | 0.789474     |
| leetcode   | easy       | 62             | 6              | 0.903226     |
| codeforces | hard       | 3              | 3              | 0            |
| codeforces | medium     | 2              | 2              | 0            |
| codeforces | easy       | 4              | 1              | 0.75         |


So we just see evaluation now, scope for improvement: 
1. using a different/larger model
2. Finetuning this model
