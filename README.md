# Lightweight LLMs for Automated Code Smell Detection

A comparison of four lightweight sub-2B LLMs — Qwen3-0.6B, Qwen3-1.7B, Llama-3.2-1B-Instruct, and Gemma-3-1B-it — fine-tuned with LoRA on the MLCQ Java dataset for binary code-smell detection (Blob, Data Class, Feature Envy, Long Method). All four train under one shared pipeline with class-weighted loss and a leakage-safe split by sample on a free Kaggle T4. Qwen3-1.7B scores the highest present-class F1 (0.60).

Course project for CMP614 Software and Paradigms, Cairo University. The paper (PDF and LaTeX source) is in `paper/`.

## Structure

```
notebooks/
  01-prepare-mlcq-dataset-kaggle.ipynb   build the cleaned dataset from raw MLCQ
  training/                              one notebook per model, same pipeline
paper/
  Paper.pdf                              compiled paper
  Latex_Files/                           LaTeX source
```

## Running it

Cleaned dataset on Kaggle: https://www.kaggle.com/datasets/fatemakotb02/mlcq-cleaned-binary-20. The notebooks run on Kaggle (T4):

```bash
pip install torch transformers peft datasets accelerate scikit-learn pandas numpy
```

Llama and Gemma are gated on Hugging Face, so you must request access and supply an HF token before their notebooks can download the model — without it those two runs fail. The trained LoRA adapters are not committed, so reproducing a result means re-running its notebook to fine-tune from scratch.

1. `notebooks/01-prepare-mlcq-dataset-kaggle.ipynb` — regenerate the cleaned dataset, or pull it from Kaggle.
2. `notebooks/training/<model>.ipynb` — fine-tune and evaluate one model; each applies LoRA (rank 8, alpha 16, dropout 0.05) and reports overall and per-smell metrics.

Seed 42 throughout.

## Results

Test-set performance (positive class: Present):

| Model | Accuracy | Precision | Recall | F1 |
|---|---|---|---|---|
| Qwen3-0.6B | 0.779 | 0.514 | 0.558 | 0.535 |
| Qwen3-1.7B | 0.753 | 0.476 | 0.820 | **0.602** |
| Llama-3.2-1B-Instruct | 0.754 | 0.463 | 0.504 | 0.483 |
| Gemma-3-1B-it | 0.745 | 0.461 | 0.712 | 0.560 |

Long Method was the easiest smell to detect, Feature Envy the hardest.

Licensed under MIT.

## Contributors

| [<img src="https://github.com/FatemaKotb.png" width="80" alt="Fatema Kotb"/><br/>Fatema Kotb](https://github.com/FatemaKotb) | [<img src="https://github.com/ahmedemad81.png" width="80" alt="Ahmed Emad"/><br/>Ahmed Emad](https://github.com/ahmedemad81) | [<img src="https://github.com/Rofaydaaa.png" width="80" alt="Rofayda Bassem"/><br/>Rofayda Bassem](https://github.com/Rofaydaaa) | [<img src="https://github.com/Mahy02.png" width="80" alt="Mahinour Elsarky"/><br/>Mahinour Elsarky](https://github.com/Mahy02) |
|:-:|:-:|:-:|:-:|