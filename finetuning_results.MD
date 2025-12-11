# Output of finetuning the model using deepspeed and accelerate:
```bash
{'loss': 0.8772, 'grad_norm': 1.4924530982971191, 'learning_rate': 8.8e-07, 'epoch': 0.96}
{'loss': 0.866, 'grad_norm': 1.4623550176620483, 'learning_rate': 8.000000000000001e-07, 'epoch': 0.96}
{'loss': 0.8542, 'grad_norm': 1.4050102233886719, 'learning_rate': 7.2e-07, 'epoch': 0.96}
{'loss': 0.901, 'grad_norm': 1.6475125551223755, 'learning_rate': 6.4e-07, 'epoch': 0.97}
{'loss': 0.8359, 'grad_norm': 1.5600303411483765, 'learning_rate': 5.6e-07, 'epoch': 0.97}
{'loss': 0.8332, 'grad_norm': 1.4388444423675537, 'learning_rate': 4.800000000000001e-07, 'epoch': 0.98}
{'loss': 0.8453, 'grad_norm': 1.3793333768844604, 'learning_rate': 4.0000000000000003e-07, 'epoch': 0.98}
{'loss': 0.842, 'grad_norm': 1.6092510223388672, 'learning_rate': 3.2e-07, 'epoch': 0.98}
{'loss': 0.891, 'grad_norm': 1.3359293937683105, 'learning_rate': 2.4000000000000003e-07, 'epoch': 0.99}
{'loss': 0.8235, 'grad_norm': 1.4160754680633545, 'learning_rate': 1.6e-07, 'epoch': 0.99}
{'loss': 0.8295, 'grad_norm': 1.587388038635254, 'learning_rate': 8e-08, 'epoch': 1.0}
{'loss': 0.864, 'grad_norm': 1.3693010807037354, 'learning_rate': 0.0, 'epoch': 1.0}
{'train_runtime': 71131.9207, 'train_samples_per_second': 0.07, 'train_steps_per_second': 0.035, 'train_loss': 0.9136463201522828, 'epoch': 1.0}
100%|████████████████████████████████████████████████████████████████████████████████████████████████████████████████| 2500/2500 [19:45:31<00:00, 28.45s/it]
Saving final model...
Saving final model...
Done! Model saved to Qwen2.5-Coder-7B-rStar-Full
```

Attempting to evaluate this model using LCB in the cloned conda environment:
python -m lcb_runner.runner.main \
    --model Qwen/Qwen2.5-Coder-7B-Instruct \
    --local_model_path /home/kbs/model_checkpoints/Qwen_Finetunes/Qwen2.5-Coder-7B-rStar-Full \
    --scenario codegeneration \
    --n 50 \
    --codegen_n 5 \
    --temperature 0.7 \
    --top_p 0.95 \
    --release_version main \
    --evaluate
