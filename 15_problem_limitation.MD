## Fixing the 15-Problem Limitation in LiveCodeBench Code Generation

If your LCB runs only evaluate **15 problems**, even when using `--n 50`, `--n 100`, or `--n -1`, the issue is **not your command or run folder**.  
It is because LCB was loading the **lite** version of the dataset.

### Root Cause
LiveCodeBench provides two datasets:

| Dataset Name | Description |
|--------------|-------------|
| `livecodebench/code_generation_lite` | Small debug subset (≈15 problems) |
| `livecodebench/code_generation` | Full benchmark dataset (hundreds/thousands of problems) |

If the runner loads `code_generation_lite`, you will **never** get more than ~15 problems, no matter what `--n` you specify.

### Solution: Edit the Dataset Loader and commit
Modify the dataset name inside the LCB runner code.

**File to edit:** lcb_runner/benchmarks/code_generation.py


**Original (lite dataset):**
```python
dataset = load_dataset("livecodebench/code_generation_lite", split="test", version_tag=release_version)
```

**Updated (dataset):**
```python
dataset = load_dataset("livecodebench/code_generation", split="test", version_tag=release_version)
```

# Faced the version_tag issue
