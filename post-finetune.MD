### Note: LiveCodeBench evaluation in environment : LCB_2_clone
  
okay I wanna do 2 things. 
1- I wanna see actual input output what the model is being finetuned with. is it being cut off?
2- maybe we can train just on input output code

# finetuning dataset check:
```python
# 1. Imports
from datasets import load_dataset
from transformers import AutoTokenizer
import matplotlib.pyplot as plt # Optional, for a histogram

# 2. Config (Match your training script)
MODEL_NAME = "Qwen/Qwen2.5-Coder-7B-Instruct"
MAX_SEQ_LENGTH = 4096  # The limit you set

# 3. Load Dataset & Tokenizer
print("Loading data and tokenizer...")
dataset = load_dataset("microsoft/rStar-Coder", "synthetic_sft", split="train[:500]") # Inspect first 500
tokenizer = AutoTokenizer.from_pretrained(MODEL_NAME)

# 4. Your Exact Formatting Function
def format_rstar_to_chat(row):
    user_content = row['question']
    if row.get('starter_code') and len(str(row['starter_code'])) > 0:
        user_content += f"\n\nHere is the starter code:\n```python\n{row['starter_code']}\n```"

    # Crucial part: Code is at the END
    assistant_content = f"{row['response']}\n\nHere is the solution:\n```python\n{row['code']}\n```"
    
    messages = [
        {"role": "system", "content": "You are an expert coding assistant. Think through the problem step-by-step before writing the code."},
        {"role": "user", "content": user_content},
        {"role": "assistant", "content": assistant_content}
    ]
    return {"messages": messages}

# 5. Analyze Lengths
print("Analyzing token lengths...")
lengths = []
truncated_samples = []

for i, row in enumerate(dataset):
    # Format message
    formatted = format_rstar_to_chat(row)
    
    # Convert to text string (what the model actually sees)
    full_text = tokenizer.apply_chat_template(formatted['messages'], tokenize=False)
    
    # Count tokens
    tokens = tokenizer(full_text)['input_ids']
    curr_len = len(tokens)
    lengths.append(curr_len)
    
    # Save "Dangerous" examples
    if curr_len > MAX_SEQ_LENGTH:
        truncated_samples.append({
            "index": i,
            "length": curr_len,
            "text": full_text,
            "last_tokens": tokenizer.decode(tokens[MAX_SEQ_LENGTH-50:MAX_SEQ_LENGTH]) # What the model sees at the cut
        })

# 6. Report
print(f"\nChecked {len(lengths)} samples.")
print(f"Samples exceeding {MAX_SEQ_LENGTH} tokens: {len(truncated_samples)} ({len(truncated_samples)/len(lengths)*100:.1f}%)")

if truncated_samples:
    print("\n⚠️ WARNING: TRUNCATION DETECTED ⚠️")
    bad_sample = truncated_samples[0]
    print(f"Example Index: {bad_sample['index']}")
    print(f"Original Length: {bad_sample['length']}")
    print(f"Model sees text ending at token {MAX_SEQ_LENGTH}. The cut-off text looks like:")
    print("-" * 40)
    print(bad_sample['last_tokens'])
    print("-" * 40)
    print("If you don't see the ```python code``` block above, your model trained on reasoning with NO ANSWERS.")
else:
    print("\n✅ SAFE. All samples fit within the limit.")
```
