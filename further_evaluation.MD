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





# code to find which file in the directory has the results
```python
python - <<'PY'
import json
import os

# --- Configuration ---
# The target directory where your result files are located
SEARCH_DIR = "."
# ---------------------

def find_evaluation_log(directory):
    """Searches for the file that contains the 'graded_list' key."""
    
    potential_files = [f for f in os.listdir(directory) if f.endswith('.json')]
    
    for filename in potential_files:
        if filename.endswith('_eval_all.json'):
            # This naming convention is the strongest hint, check it first
            file_path = os.path.join(directory, filename)
            try:
                with open(file_path, 'r') as f:
                    data = json.load(f)
                
                # Check for the key that only exists in the detailed evaluation log
                if isinstance(data, list) and data and 'graded_list' in data[0]:
                    print("=" * 80)
                    print(f"✅ FOUND THE REQUIRED FILE: {filename}")
                    print("This file contains the Pass/Fail results for every problem and is needed for analysis.")
                    print("=" * 80)
                    return filename
            except Exception:
                # Ignore files that aren't valid JSON or are corrupted
                continue
    
    print("\n Could not automatically find the required '*_eval_all.json' file.")
    print("Please ensure you ran the evaluation step (`--evaluate`) to generate the detailed graded results.")
    return None

find_evaluation_log(SEARCH_DIR)

PY
```

# code for getting results 
change directory name accordingly

```python
python - <<'PY'
import json
import pandas as pd
import os

FILE_PATH = "output/Qwen2.5-Coder-Ins-7B_10stimeout/Scenario.codegeneration_10_0.2_eval_all.json"

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
print("--- Failure Analysis by Platform and Difficulty (10s TIMEOUT DATA) ---")
print("="*80)
print(failed_analysis.to_markdown(index=False, floatfmt=('.0f', '.0f', '.0f', '.2%')))
PY
```
