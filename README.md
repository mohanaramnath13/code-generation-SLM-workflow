# Order of files

## 1.setup
## 2.setup2
## 3.running (theory to read)
## 4.15_problem_limitation
## 5.version_tag_syntax
## 6.dataset_revision_issue.MD
## 7.date_and_filterbydate.MD

Final command : do not use --debug as it limits the number of problems
``` python
python -m lcb_runner.runner.main \
    --model Qwen/Qwen2.5-Coder-7B-Instruct \
    --scenario codegeneration \
    --n 50 \
    --codegen_n 5 \
    --temperature 0.7 \
    --top_p 0.95 \
    --release_version main \
    --custom_output_save_name qwen2.5_coder_7b_pass5_50
```
