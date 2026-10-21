---
name: pytorch
description: Use when training or serving small neural networks with PyTorch on CPU — installing CPU-only wheels, reproducible training loops, data loaders, image classifiers (e.g. FashionMNIST) and text classifiers on character n-grams, distilling a large local model's labels into a small fast model, evaluation with macro F1, and saving models for serving. Also use when installs pull gigabytes of CUDA libraries, training is not reproducible, or accuracy looks good but some classes are never predicted.
---

# PyTorch (CPU-first)

Default: CPU wheels, a plain training loop you can read, fixed seeds, a held-out test set, metrics per class, models tracked with an experiment tracker.

## Install without GPU baggage

```bash
uv add torch --index https://download.pytorch.org/whl/cpu
```

Verify the lock file contains no `nvidia-*`, `triton` or CUDA packages. On shared machines cap threads: `torch.set_num_threads(2)`.

## Reproducible training loop

```python
torch.manual_seed(SEED); random.seed(SEED); np.random.seed(SEED)
for epoch in range(EPOCHS):
    model.train()
    for x, y in train_loader:
        opt.zero_grad()
        loss = loss_fn(model(x), y)
        loss.backward()
        opt.step()
    model.eval()
    with torch.no_grad():
        evaluate(model, val_loader)
```

- Set a time budget (e.g. stop after N minutes) as well as an epoch count, so jobs on modest machines finish.
- Log hyperparameters, seed, data version and metrics for every run.

## Distillation from a big local model

1. Label a sample with one or two large models (teachers); keep only items where teachers agree.
2. Check the label distribution **before** training. If one class dominates (e.g. 95 % of items), accuracy is meaningless — collect or generate items for the missing classes first.
3. Train a small student (character n-gram embeddings + a small CNN or MLP works well for short texts in any script).
4. Report agreement with the teacher on a held-out set, **macro F1**, per-class recall, and per-item latency for both teacher and student.

## Evaluation

- Use a stratified split; keep a test set you never tune on.
- Macro F1 and a confusion matrix reveal classes the model ignores; accuracy hides them.
- Compare with a trivial baseline (majority class, logistic regression on the same features).

## Saving and serving

- Save `state_dict` plus the code version and preprocessing parameters; or export with TorchScript/ONNX for serving.
- Wrap inference in a function that takes raw input and applies the same preprocessing as training.

## Verification loop

1. Training twice with the same seed yields the same metrics (within tiny tolerance on CPU).
2. Test metrics recorded with the run.
3. A smoke test loads the saved model and predicts on a few known inputs.

## Pitfalls

- **Forgetting `model.eval()`** keeps dropout and batch-norm in training mode during evaluation.
- **Data leakage:** deduplicate before splitting; near-duplicates across train and test inflate scores.
- **`num_workers > 0`** in data loaders on some platforms needs the `if __name__ == "__main__":` guard.
- **Shape bugs** broadcast silently; assert tensor shapes at boundaries.
- **Class imbalance:** use class weights or resampling, and report per-class metrics.
