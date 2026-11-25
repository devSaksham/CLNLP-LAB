# IMDB Sentiment Analysis: Comparative Study of LR, LSTM, Transformer and BERT

Binary sentiment classification (positive / negative) on the 50K IMDB Movie Reviews dataset, comparing a classical baseline, two neural models trained from scratch, and a pretrained language model.

## Models

| # | Model | Features / Input | Training |
|---|---|---|---|
| 1 | Logistic Regression | TF-IDF (unigrams + bigrams, 50K features) | scikit-learn, CPU |
| 2 | Bidirectional LSTM | Word embeddings (vocab 20K, max length 256) | PyTorch, from scratch |
| 3 | Vanilla Transformer | Embeddings + sinusoidal positional encoding, 2 encoder layers, 4 heads | PyTorch, from scratch |
| 4 | BERT (`bert-base-uncased`) | WordPiece tokens, max length 256 | Hugging Face, fine-tuned |

## Evaluation

All models are scored on the same held-out test set using:

- Accuracy
- Precision
- Recall
- F1 score
- Confusion Matrix (TP, TN, FP, FN, plus false positive and false negative rates)

## Dataset

- Kaggle: https://www.kaggle.com/datasets/lakshmi25npathi/imdb-dataset-of-50k-movie-reviews
- Reference code: https://github.com/Ankit152/IMDB-sentiment-analysis/tree/master
- File used: `IMDB Dataset.csv` (columns `review`, `sentiment`; 25K positive, 25K negative)

Place `IMDB Dataset.csv` next to the notebook. If it is missing, the notebook downloads it automatically with `kagglehub`.

**Split (stratified):** 70% train (35,000), 10% validation (5,000), 20% test (10,000). The validation set is used to pick the best epoch for the neural models. The test set is used only once, for the final scores.

## Repository Layout

```
.
├── imdb_sentiment_analysis.ipynb   # full experiment
├── README.md
└── IMDB Dataset.csv                # download from Kaggle (or auto-downloaded)
```

## Setup

Python 3.9 or newer is recommended. A GPU is strongly recommended for BERT (Google Colab or Kaggle free GPU is enough).

```bash
pip install torch transformers scikit-learn pandas numpy matplotlib seaborn tqdm kagglehub jupyter
```

## How to Run

```bash
jupyter notebook imdb_sentiment_analysis.ipynb
```

Then use **Run All**. If using Colab or Kaggle, upload the notebook, enable a GPU runtime, and uncomment the `pip install` cell.

For a quick dry run, edit the configuration cell:

```python
SAMPLE_SIZE     = 10000   # use a 10K subset of the data
BERT_TRAIN_SIZE = 5000    # fine-tune BERT on fewer samples
EPOCHS          = 2
BERT_EPOCHS     = 1
```

## Notebook Structure

| Section | Description |
|---|---|
| 1. Setup and Configuration | Imports, hyperparameters, random seed, device selection |
| 2. Data Loading and Preprocessing | Load CSV, strip HTML, normalize text, stratified split |
| 3. Shared Utilities | Metric function, confusion matrix and training curve plots, generic training and prediction loops |
| 4. Logistic Regression | TF-IDF vectorizer and classifier |
| 5. Tokenization | Vocabulary built from the training split, padding and truncation |
| 6. LSTM | Model definition, training, evaluation |
| 7. Vanilla Transformer | Model definition, training, evaluation |
| 8. BERT | Tokenization, fine-tuning loop with mixed precision, evaluation |
| 9. Comparative Analysis | Results table, bar chart, confusion matrices, error rates, best model summary, discussion |

## Key Configuration

| Parameter | Default | Meaning |
|---|---|---|
| `SEED` | 42 | Reproducibility |
| `MAX_LEN` | 256 | Token limit for LSTM and Transformer |
| `VOCAB_SIZE` | 20000 | Vocabulary size for scratch models |
| `BATCH_SIZE` | 64 | Batch size for scratch models |
| `EPOCHS` | 5 | Epochs for LSTM and Transformer |
| `BERT_MAX_LEN` | 256 | Token limit for BERT |
| `BERT_BATCH_SIZE` | 32 | Reduce to 16 if GPU memory is limited |
| `BERT_EPOCHS` | 2 | Fine-tuning epochs |
| `BERT_LR` | 2e-5 | BERT learning rate |

## Expected Behaviour

Exact numbers depend on hardware, seed and settings, but typical outcomes are roughly:

| Model | Approx. F1 | Relative Cost |
|---|---|---|
| Logistic Regression | 0.89 to 0.90 | Seconds on CPU |
| LSTM | 0.86 to 0.89 | Minutes |
| Vanilla Transformer | 0.83 to 0.87 | Minutes |
| BERT | 0.92 to 0.94 | Longest, needs GPU |

General trend: BERT is best, the TF-IDF baseline is competitive with the from-scratch neural models, and the from-scratch Transformer tends to underperform because it lacks pretraining. The notebook prints the real numbers and an automatic ranking at the end.

## Outputs Produced

- Per-model classification report (precision, recall, F1 per class)
- Training and validation loss curves, and validation F1 per epoch (LSTM, Transformer, BERT)
- Confusion matrix for every model, plus a side by side comparison
- Summary table and grouped bar chart of Accuracy, Precision, Recall, F1
- Training time per model
- FP and FN rate table for error analysis

## Troubleshooting

| Problem | Fix |
|---|---|
| `CUDA out of memory` during BERT | Lower `BERT_BATCH_SIZE` to 16 or 8, or `BERT_MAX_LEN` to 128 |
| BERT is too slow | Set `BERT_TRAIN_SIZE` (for example 10000) and `BERT_EPOCHS = 1`, or use a GPU runtime |
| `FileNotFoundError` for the CSV | Place `IMDB Dataset.csv` beside the notebook, or ensure internet access so `kagglehub` can download it |
| Kaggle download fails | Download the CSV manually from the dataset link above |
| Hugging Face download blocked | Check internet access, or pre-download `bert-base-uncased` and set `BERT_NAME` to the local folder |

## Notes

- Preprocessing differs by model family: LR, LSTM and Transformer use lowercased, punctuation-stripped text; BERT uses HTML-stripped text that keeps casing and punctuation, which suits its tokenizer.
- The vocabulary and TF-IDF vectorizer are fitted on the training split only, so no test information leaks into training.
- The best validation epoch is restored before testing for every neural model.
