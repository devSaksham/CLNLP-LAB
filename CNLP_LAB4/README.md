# Experiment 4 — Term Frequency Analysis and Named Entity Recognition

## Overview
This project implements all three parts of Experiment 4:

- **4.1** — Term Frequency (TF) analysis and Named Entity Recognition (NER) on a text file, using the **spaCy** NLP toolkit.
- **4.2** — Term Frequency (TF) analysis on the same text file, implemented in pure Python with **no NLP toolkit**.
- **4.3** — TF-IDF calculation across three sample documents, implemented entirely from scratch (no NLTK, spaCy, or any other NLP toolkit).

## Files

| File | Description |
|---|---|
| `experiment4.ipynb` | Main Jupyter notebook containing all three parts |
| `4.1_4.2_input.txt` | Input text file used by Parts 4.1 and 4.2 |
| `README.md` | This file |

### Files generated after running the notebook

| File | Description |
|---|---|
| `4.1_term_frequency.csv` | Term, Frequency table (Part 4.1, spaCy) |
| `4.1_named_entities.csv` | Entity, Label table (Part 4.1, NER) |
| `4.2_term_frequency.csv` | Term, Frequency table (Part 4.2, no toolkit) |
| `4.3_tfidf_scores.csv` | Document, Term, TF, DF, IDF, TF-IDF table (Part 4.3) |

## Requirements

```bash
pip install spacy pandas matplotlib
python -m spacy download en_core_web_sm
```

## How to Run

1. Keep `experiment4.ipynb` and `4.1_4.2_input.txt` in the same folder.
2. Install the requirements above.
3. Open the notebook in Jupyter Notebook, JupyterLab, VS Code, or Google Colab.
4. Run all cells in order (Part 4.1 → Part 4.2 → Part 4.3).
5. CSV outputs will be saved automatically in the same folder as the notebook.

## Notes on Approach

- **Part 4.1** uses spaCy for tokenization (keeping only `token.is_alpha` tokens, lower-cased, to drop punctuation/numbers) and its pretrained NER model (`en_core_web_sm`) to detect entities such as people, organizations, locations, and dates.
- **Part 4.2** replicates the same normalization (lowercase + punctuation removal) using only Python's built-in `string` module and dictionary operations — no NLP library is involved anywhere in this section.
- **Part 4.3** implements the standard TF-IDF pipeline from scratch:
  - `TF(term, doc) = count(term, doc) / total_terms(doc)`
  - `DF(term) = number of documents containing term`
  - `IDF(term) = log10(N / DF(term))`, where N = total number of documents
  - `TF-IDF(term, doc) = TF(term, doc) × IDF(term)`

  Each step's output (TF table, DF table, IDF table, and the combined TF-IDF summary) is displayed separately in the notebook, followed by the top 10 highest-scoring terms per document.

## Verified

The notebook was executed end-to-end with `jupyter nbconvert --execute` and all cells ran without errors, producing all four CSV output files.
