### Fixing Data Formatting Crashes and Hidden Dataset Filtering in LiveCodeBench

During execution of the full `livecodebench/code_generation` dataset, two major issues were discovered and resolved: a data formatting crash caused by missing dates, and an internal filtering bug that reduced the dataset to only 15 problems even when the full 400+ were loaded.

---

## 1. Fix: Data Formatting Crash (`AttributeError: 'NoneType' object has no attribute 'isoformat'`)

Some benchmark entries have `contest_date = None`.  
The original LCB code assumed all dates were valid ISO strings or datetime objects.  
This caused two separate crashes:

### 1.1 Crash During Data Loading (`__post_init__`)
**Original issue:**  
`self.contest_date = datetime.fromisoformat(self.contest_date)`  
would crash if `contest_date` was `None`.

**Fix implemented:**  
Added a safe conditional check so parsing only happens when the field is a valid string.

### 1.2 Crash During Output Saving (`insert_output`)
**Original issue:**  
`"contest_date": self.contest_date.isoformat()`  
always assumed a valid datetime object, causing a crash when `None`.

**Fix implemented:**  
Added safe serialization:

```python
"contest_date": self.contest_date.isoformat() if self.contest_date is not None else None
  ```


### 2.Fix: Internal Filtering That Limited the Dataset to 15 Problems

Even after successfully loading the full `livecodebench/code_generation` dataset (≈400 problems), the LiveCodeBench runner was only processing **15 problems**, ignoring flags like `--n 50` or `--n 100`. This issue was caused by hidden filtering logic inside the dataset loader.

#### Root Cause
Inside the `load_code_generation_dataset` function, the code contained date-range filtering intended to limit problems by `start_date` and `end_date`. However, due to incorrect handling of `None` defaults, this logic unintentionally filtered out almost the entire dataset, leaving only ~15 entries.

**Problematic code block:**
```python
if start_date is not None:
    p_start_date = datetime.strptime(start_date, "%Y-%m-%d")
    dataset = [e for e in dataset if p_start_date <= e.contest_date]

if end_date is not None:
    p_end_date = datetime.strptime(end_date, "%Y-%m-%d")
    dataset = [e for e in dataset if e.contest_date <= p_end_date]
```

Even when no dates were provided, the logic was still affecting the dataset because of how contest_date was parsed and stored earlier.

Fix Implemented

The entire filtering block was disabled (commented out) to ensure that LiveCodeBench passes the full dataset to the runner:
```python
# if start_date is not None:
#     p_start_date = datetime.strptime(start_date, "%Y-%m-%d")
#     dataset = [e for e in dataset if p_start_date <= e.contest_date]

# if end_date is not None:
#     p_end_date = datetime.strptime(end_date, "%Y-%m-%d")
#     dataset = [e for e in dataset if e.contest_date <= p_end_date]

```
Result

-> The dataset now loads with the full ~400 problems.

-> The runner correctly respects --n (e.g., 50, 100, or full dataset).

-> No unintended filtering occurs during preprocessing.
