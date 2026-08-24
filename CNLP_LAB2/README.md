# Experiment 2: Basic Text Preprocessing

## Overview

This experiment covers the foundational steps of preparing raw text for use in
Natural Language Processing (NLP) pipelines. Before any model or algorithm can
work with text, the text needs to be cleaned and broken down into smaller,
consistent units — that's what this experiment demonstrates, in three parts.

## What's Being Done

### 2.1 — Basic Text Preprocessing
Takes a raw paragraph of text and cleans it step by step:
- **Lowercasing** — normalizes case so "NLP" and "nlp" are treated the same;
  also counts how many uppercase letters were in the original text.
- **Punctuation removal** — strips out symbols like `.`, `,`, `!`, `(`, `)`;
  also reports how many distinct punctuation marks appeared.
- **Number removal** — strips out digits; also reports how many numbers
  (as whole numeric tokens, e.g. "2026") were found.
- **Extra whitespace removal** — collapses multiple spaces, tabs, and blank
  lines into single spaces; also measures how much extra whitespace was
  present before cleanup.
- **Final output** — the fully cleaned text, ready for further processing.

This shows why preprocessing matters: raw text is messy, and models perform
better on normalized, consistent input.

### 2.2 — Word & Sentence Tokenization
Takes a paragraph and breaks it into sentences and words, using three
different approaches so the methods can be compared:
- **NLTK** — uses a pre-trained statistical model (Punkt) to split text into
  sentences and words, handling tricky cases like abbreviations correctly.
- **spaCy** — uses a full pre-trained language pipeline; tokenization here is
  one part of a larger model that also understands grammar and structure.
- **Manual (no library)** — uses plain Python regular expressions to split
  text into sentences and words from scratch, showing what NLTK/spaCy are
  doing "under the hood" using simple rules instead of trained models.

Comparing all three highlights the trade-off between simple rule-based
tokenization and smarter, model-based tokenization.

### 2.3 — Stop Word Removal
Takes tokenized text and removes common "filler" words (like *is*, *the*,
*a*, *an*, *in*, *of*, *and*, *to*) that carry little meaning on their own
and are usually discarded before deeper text analysis. The experiment
displays three things side by side:
- the original tokens,
- the filtered tokens (stop words removed),
- and the stop words that were found and removed.

This step is common in search engines, text classification, and topic
modeling, where reducing noise words improves results.

## Why This Matters

Preprocessing is the first stage of almost every NLP pipeline. Cleaning text
and breaking it into tokens directly affects the quality of everything built
on top of it — search, sentiment analysis, chatbots, translation, and
summarization all depend on well-preprocessed input.

## Files

| File | Description |
|---|---|
| `Experiment_2_Text_Preprocessing.ipynb` | Jupyter notebook containing all code for 2.1, 2.2, and 2.3 |
| `content/2.1_text_data.txt` | Sample paragraph used for preprocessing (2.1) |
| `content/2.2_tokenization_data.txt` | Sample paragraph used for tokenization (2.2) |
| `content/2.3_clean_data.txt` | Sample paragraph used for stop word removal (2.3) |

> The notebook's setup cell creates the `content/` folder and sample files
> automatically, so it runs standalone without needing them uploaded first.

## Requirements

- Python 3.x
- `nltk` (with `punkt`, `punkt_tab`, and `stopwords` downloaded)
- `spacy` (with the `en_core_web_sm` model downloaded)

Install with:
```bash
pip install nltk spacy
python -m spacy download en_core_web_sm
```

## How to Run

1. Open `Experiment_2_Text_Preprocessing.ipynb` in Jupyter or Google Colab.
2. Run the setup cell first to create the sample data files.
3. Run the remaining cells in order — each section (2.1, 2.2, 2.3) is
   self-contained and prints its own labeled output.
