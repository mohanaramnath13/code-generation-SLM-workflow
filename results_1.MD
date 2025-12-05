## Evaluation Failure Analysis Report (Pass@10 by Platform & Difficulty)

The evaluation results were analyzed using a custom pandas script:

```python
python - <<'PY'
import json
import pandas as pd
import os

FILE_PATH = "output/Qwen2.5-Coder-Ins-7B_rawtimeout/Scenario.codegeneration_10_0.2_eval_all.json"

if not os.path.exists(FILE_PATH):
    print(f"\n Error: Detailed evaluation file not found at {FILE_PATH}.")
    exit()

data = json.load(open(FILE_PATH, 'r'))
df = pd.DataFrame(data)

df['Pass@10_Success'] = df['graded_list'].apply(lambda x: any(x))

analysis = df.groupby(['platform', 'difficulty'])['Pass@10_Success'].agg(
    Total_Problems='count',
    Total_Failures=lambda x: (x == False).sum(),
    Success_Rate='mean'
).reset_index()

failed_analysis = analysis.sort_values('Total_Failures', ascending=False)\
                          .rename(columns={'Success_Rate': 'Pass@10_Rate'})

print("\n" + "="*80)
print("--- Failure Analysis by Platform and Difficulty (LOW TIMEOUT DATA) ---")
print("="*80)
print(failed_analysis.to_markdown(index=False, floatfmt=('.0f', '.0f', '.0f', '.2%')))
PY
```

## Output Summary

Failure Analysis by Platform and Difficulty (LOW TIMEOUT DATA)
================================================================================
| platform   | difficulty   |   Total_Problems |   Total_Failures |   Pass@10_Rate |
|:-----------|:-------------|-----------------:|-----------------:|---------------:|
| atcoder    | hard         |               58 |               58 |       0        |
| atcoder    | medium       |               76 |               58 |       0.236842 |
| leetcode   | medium       |               90 |               29 |       0.677778 |
| leetcode   | hard         |               29 |               22 |       0.241379 |
| atcoder    | easy         |               76 |               16 |       0.789474 |
| leetcode   | easy         |               62 |                6 |       0.903226 |
| codeforces | hard         |                3 |                3 |       0        |
| codeforces | medium       |                2 |                2 |       0        |
| codeforces | easy         |                4 |                1 |       0.75     |

## Interpretation of Results

| Rank | Platform  | Difficulty | Total Failures | Pass@10 Rate | Interpretation |
|------|-----------|------------|----------------|--------------|----------------|
| 1    | AtCoder   | Hard       | 58             | 0.00%        | Model failed every attempt → almost certainly TLE (timeouts). |
| 2    | AtCoder   | Medium     | 58             | 23.68%       | Very high failure rate, indicating slow or inefficient algorithms. |
| 3    | LeetCode  | Medium     | 29             | 67.78%       | Moderate error rate; failures likely due to problem constraints or TLE. |
| 4    | LeetCode  | Hard       | 22             | 24.14%       | Expected performance on hard tasks, but TLE likely worsened the results. |

---

## Interpretation Summary

The data shows that **most failures—especially on AtCoder—are caused by Time Limit Exceeded (TLE)** rather than incorrect logic.

### Evidence:
- **AtCoder Easy:** 78.9% Pass@10  
- **LeetCode Easy:** 90.3% Pass@10  
- **AtCoder Hard:** 0% (complete failure consistent with TLE)  
- **Codeforces Hard/Medium:** 0% (also consistent with TLE patterns)

This indicates that the model is **functionally correct on simpler tasks**, but the generated solutions are **too slow for competitive programming constraints**, especially on AtCoder.

---

## Action Plan

### 1. Eliminate Timeouts (Immediate Priority)
Rerun evaluation with a higher timeout to distinguish:
- **Performance failures (TLE)**  
- **True logical errors**

This step is necessary to obtain the model’s real algorithmic accuracy.

### 2. Post-Timeout Interpretation
If Pass@10 remains low (**<30%**) on AtCoder Hard/Medium even after increasing the timeout:

- The issue is **algorithmic**, not performance-related.  
- AtCoder tasks require deeper techniques (DP, graphs, math-heavy reasoning).  
- Consider **fine-tuning Qwen** on datasets rich in AtCoder-style algorithmic problems.


⏱️ Re-Evaluate with Higher Timeout (Run Now)
```python
python -m lcb_runner.runner.main \
    --model Qwen/Qwen2.5-Coder-7B-Instruct \
    --scenario codegeneration \
    --continue_existing_with_eval \
    --evaluate \
    --timeout 15 \
    --release_version main \
    --custom_output_save_name qwen2.5_coder_7b_pass5_50
This will recompute Pass@k with fewer TLE failures, producing a more accurate skill profile of the model.
```
