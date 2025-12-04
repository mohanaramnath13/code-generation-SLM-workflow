### Fixing Dataset Revision Issues in LiveCodeBench

The LCB runner was failing to load the full dataset due to an incorrect default revision tag, resulting in errors like:
  DatasetNotFoundError: Revision 'release_latest' doesn't exist...


**Fix:** Explicitly specify the correct dataset revision using the `--release_version main` flag.  
This ensures the full `livecodebench/code_generation` dataset loads successfully, bypassing all version mismatch issues.

**Final stable execution command:**

```bash
python -m lcb_runner.runner.main \
    --model Qwen/Qwen2.5-Coder-7B-Instruct \
    --scenario codegeneration \
    --n 50 \
    --codegen_n 5 \
    --temperature 0.7 \
    --top_p 0.95 \
    --debug \
    --release_version main \
    --custom_output_save_name qwen2.5_coder_7b_pass5_50
