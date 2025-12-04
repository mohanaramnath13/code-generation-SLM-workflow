### Fixing the `version_tag` Error in LiveCodeBench

If LiveCodeBench throws:  
`TypeError: load_dataset() got an unexpected keyword argument 'version_tag'`  
the dataset loader is using the deprecated `version_tag` parameter. HuggingFace `datasets` removed this argument, so it must be replaced with the standard `revision` parameter.

**File to edit:**  
`lcb_runner/benchmarks/code_generation.py`

**Find and replace:**

**Old:**
```python
dataset = load_dataset("livecodebench/code_generation",
                       split="test",
                       version_tag="release_v2")
```

**New:**
```python
dataset = load_dataset("livecodebench/code_generation",
                       split="test",
                       revision="release_v2")
```

If using the CLI argument (--release_version):
```python
dataset = load_dataset("livecodebench/code_generation",
                       split="test",
                       revision=release_version)
```

commit the change:
git add lcb_runner/benchmarks/code_generation.py
git commit -m "FIX: Replace deprecated 'version_tag' with 'revision' in dataset loading."
git push origin main

