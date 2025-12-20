### Fine-tuning Update: Addressing Token Truncation
**Issue Identified:** Initial evaluation revealed that while the model successfully generated reasoning traces, it failed to output the final code solution.
**Root Cause:** The `rStar-Coder` dataset contains reasoning chains exceeding 13,000 tokens. With our context limit set to 4,096, approximately **88% of the dataset was truncated**, effectively stripping the ground-truth code from the end of the training examples.
**Resolution:** The pipeline has been pivoted to fine-tune exclusively on **Input-Code pairs** (Direct Code). This ensures all training examples fit within the memory constraints.
**Next Steps:** Training has been relaunched with the corrected setup. Future iterations may explore "Middle Truncation" or expanded context windows to re-integrate Chain-of-Thought capabilities.

```bash
{'loss': 1.042, 'grad_norm': 970.0249633789062, 'learning_rate': 1.98e-05, 'epoch': 0.01}
{'loss': 0.7193, 'grad_norm': 5.20620059967041, 'learning_rate': 1.9600000000000002e-05, 'epoch': 0.02}
{'loss': 0.712, 'grad_norm': 6.233624458312988, 'learning_rate': 1.94e-05, 'epoch': 0.03}
{'loss': 0.6273, 'grad_norm': 5.105684757232666, 'learning_rate': 1.9200000000000003e-05, 'epoch': 0.04}
{'loss': 0.5887, 'grad_norm': 3.496194362640381, 'learning_rate': 1.9e-05, 'epoch': 0.05}
{'loss': 0.6008, 'grad_norm': 3.4985239505767822, 'learning_rate': 1.88e-05, 'epoch': 0.06}
.
.
{'loss': 0.4851, 'grad_norm': 2.9812474250793457, 'learning_rate': 6.000000000000001e-07, 'epoch': 0.97}
{'loss': 0.4938, 'grad_norm': 2.8866636753082275, 'learning_rate': 4.0000000000000003e-07, 'epoch': 0.98}
{'loss': 0.4845, 'grad_norm': 2.1113312244415283, 'learning_rate': 2.0000000000000002e-07, 'epoch': 0.99}
{'loss': 0.472, 'grad_norm': 3.729652166366577, 'learning_rate': 0.0, 'epoch': 1.0}
{'train_runtime': 24832.6819, 'train_samples_per_second': 0.081, 'train_steps_per_second': 0.04, 'train_loss': 0.5538048386573792, 'epoch': 1.0}
100%|█████████████████████████████████████████████████████████████████████████████████████████████████████████████████| 1000/1000 [6:53:52<00:00, 24.83s/it]
Saving final model...
Saving final model...
Done! Model saved to Qwen2.5-Coder-7B-base-rStar-I2O-2000
```

## The loss seems to have reduced. Now we try to evaluate using LiveCodeBench and compare.
