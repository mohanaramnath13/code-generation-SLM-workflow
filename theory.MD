# Understanding pass@k, Temperature, Top-p, and Top-k

## 1. pass@k
`pass@k` measures the probability that **at least one** of the model’s **k generated solutions** for a coding problem is correct.

- You generate **k samples** for the same problem.
- Run each sample against test cases.
- If **any** of the k samples pass → the model is counted correct for that problem.

This metric is useful because a model may fail on the first attempt but succeed in later attempts.

**Example**

| Samples | Correct? | pass@1 | pass@5 |
|--------|-----------|--------|--------|
| [A] | Wrong | 0 | – |
| [A, B, C, D, E] | Only C is correct | 0 | 1 |

---

## 2. Temperature
`temperature` controls randomness in token generation.

- **0.0–0.2** → deterministic, stable  
- **0.4–0.7** → balanced diversity (recommended for pass@k)  
- **0.8–1.2** → very random, often worse for code

For pass@k evaluation, **temperature must be > 0**, otherwise all k samples will be identical.

---

## 3. Top-p (Nucleus Sampling)
`top-p` keeps only the **smallest set of tokens** whose cumulative probability is at least *p*.

Example: if the model predicts the next token with probabilities:

