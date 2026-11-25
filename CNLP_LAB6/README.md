# Experiment 6: GloVe Word Embeddings

## Aim
To implement the GloVe (Global Vectors for Word Representation) algorithm to generate word embeddings from a text corpus and find the most similar words to **"king"**.

## Files
| File | Description |
|------|-------------|
| `Experiment_6_GloVe.ipynb` | Notebook containing both parts of the experiment |
| `glove_corpus.txt` | Small text corpus (created automatically by the notebook) |
| `README.md` | This file |

## Theory
GloVe is an unsupervised algorithm that learns word vectors from global word-word co-occurrence statistics.

1. Build a co-occurrence matrix `X`, where `X_ij` is how often word `j` appears in the context of word `i`.
2. Learn vectors so that their dot product approximates the log of the co-occurrence count:

   `w_i . w~_j + b_i + b~_j = log(X_ij)`

3. Minimise the weighted least squares loss:

   `J = sum over i,j of f(X_ij) * (w_i . w~_j + b_i + b~_j - log X_ij)^2`

   where `f(x) = (x / x_max)^alpha` if `x < x_max`, otherwise `1`.
4. The final embedding of a word is `w_i + w~_i`.
5. Similar words are found using **cosine similarity**.

## Notebook Structure
**Part 1: GloVe from scratch (no pretrained model)**
1. Import libraries
2. Create the corpus file
3. Load and preprocess the text
4. Build the vocabulary
5. Build the co-occurrence matrix (window size 4, distance weighted)
6. Initialise parameters
7. Define the weighting function and training loop (AdaGrad)
8. Train the model
9. Plot the training loss
10. Compute final embeddings
11. Find the most similar words to "king"
12. Visualise embeddings in 2D using PCA

**Part 2: Pretrained GloVe using Gensim**
1. Install and import Gensim
2. Load `glove-wiki-gigaword-50`
3. Find the most similar words to "king"
4. Extra checks: similarity scores and the analogy `king - man + woman`

## Requirements
- Python 3.8+
- numpy
- matplotlib
- gensim (Part 2 only)

```bash
pip install numpy matplotlib gensim jupyter
```

## How to Run
```bash
jupyter notebook Experiment_6_GloVe.ipynb
```
Run all cells from top to bottom. The notebook also runs directly on Google Colab.

Note: Part 2 downloads the pretrained model (about 66 MB) the first time, so an internet connection is needed.

## Hyperparameters (Part 1)
| Parameter | Value |
|-----------|-------|
| Window size | 4 |
| Embedding dimension | 20 |
| x_max | 10 |
| alpha | 0.75 |
| Learning rate | 0.05 |
| Epochs | 300 |

## Expected Output
- **Part 1:** Words that share context with "king" (for example `queen`, `rules`, `princess`) appear among the top results. Since the corpus has only 24 sentences, frequent words like `the` may also rank high, and results vary slightly with the random seed.
- **Part 2:** Meaningful neighbours such as `prince`, `queen`, `son`, `brother`, and the analogy `king - man + woman` gives `queen`.

## Conclusion
GloVe captures semantic relationships from co-occurrence statistics. A small corpus gives only rough similarities, while pretrained vectors trained on large corpora give far more accurate and meaningful results.
