## finetuned instruct model with 2000 examples to avoid end token problem possibly, and to hopefully get better results.
## evaluating finetuned instruct model
```
        "pass@1": 0.23425,
        "pass@5": 0.29324404761904765,
        "pass@10": 0.31
```

```
--- Failure Analysis by Platform and Difficulty (10s TIMEOUT DATA) ---
================================================================================
| platform   | difficulty   |   Total_Problems |   Total_Failures |   Pass@10_Rate |
|:-----------|:-------------|-----------------:|-----------------:|---------------:|
| leetcode   | medium       |               90 |               70 |      0.222222  |
| atcoder    | medium       |               76 |               67 |      0.118421  |
| atcoder    | hard         |               58 |               57 |      0.0172414 |
| leetcode   | hard         |               29 |               28 |      0.0344828 |
| atcoder    | easy         |               76 |               27 |      0.644737  |
| leetcode   | easy         |               62 |               19 |      0.693548  |
| codeforces | easy         |                4 |                3 |      0.25      |
| codeforces | hard         |                3 |                3 |      0         |
| codeforces | medium       |                2 |                2 |      0         |
```
