# Experiment 3: Stemming, Lemmatization and Regular Expressions

## Overview

This notebook implements three sub-experiments in Natural Language Processing text preprocessing:

| # | Task | Technique | Library |
|---|------|-----------|---------|
| 3.1 | Stemming | Porter Stemmer | `nltk.stem.PorterStemmer` |
| 3.2 | Lemmatization | POS-aware WordNet Lemmatizer | `nltk.stem.WordNetLemmatizer` |
| 3.3 | Information Extraction | Regular Expressions | `re` (built-in) |

## Files

- `Experiment3_Stemming_Lemmatization_Regex.ipynb` — the notebook (outputs pre-run, so you can read results without executing anything)
- `README.md` — this file

## Requirements

- Python 3.8+
- `nltk`

Install with:

```bash
pip install nltk
```

The notebook downloads the required NLTK data packages on first run (`punkt`, `punkt_tab`, `wordnet`, `omw-1.4`, `averaged_perceptron_tagger`, `averaged_perceptron_tagger_eng`). This needs an internet connection the first time; after that the data is cached locally.

## How to Run

1. Open the notebook in Jupyter, JupyterLab, VS Code, or Google Colab.
2. Run all cells top to bottom (**Kernel → Restart & Run All**, or `Cell → Run All`).
3. The NLTK download cell only needs to succeed once per machine — after that it will report the packages are already up to date.

## What Each Section Does

### 3.1 — Stemming
Applies the **Porter Stemmer** to a fixed word list (`playing`, `played`, `plays`, `studies`, `studying`, `connected`, `connection`, `computers`) and prints each word next to its stem. Stemming is a fast, rule-based suffix-stripping process, so stems are not always real dictionary words (e.g. `studies → studi`).

### 3.2 — Lemmatization
Applies the **WordNet Lemmatizer** to a fixed word list (`cats`, `dogs`, `running`, `runs`, `ran`, `studies`, `studying`, `better`, `children`, `mice`, `went`, `ate`, `leaves`, `caring`).

Each word is first POS-tagged with `nltk.pos_tag`, the tag is mapped to the WordNet POS format (noun/verb/adjective/adverb), and the word is lemmatized using that POS. This matters because the correct lemma depends on grammatical role (e.g. `leaves` → `leaf` as a noun, but `leave` as a verb).

**Known limitation:** because each word is tagged in isolation rather than in a sentence, the POS tagger can occasionally mistag a word (e.g. `ate` may be tagged as an adjective instead of a verb), which changes its lemma. This is a genuine limitation of POS taggers and is worth noting in the experiment writeup.

### 3.3 — Regex-based Information Extraction
Extracts five entity types from a sample paragraph of workshop text:
- **Emails** — e.g. `nlpworkshop@gmail.com`
- **URLs** — both `https://...` and bare `www....` forms
- **Mobile numbers** — 10-digit numbers, with or without an `+91` prefix
- **Hashtags** — `#word`
- **Mentions** — `@username` (using a negative lookbehind so the `@` inside an email address like `name@gmail.com` is not wrongly picked up as a mention)

## Expected Output (abridged)

**Stemming**
```
playing        -> play
studies        -> studi
connection     -> connect
```

**Lemmatization**
```
running   (VBG) -> run
went      (VBD) -> go
leaves    (NNS) -> leaf
```

**Regex extraction**
```
Emails:   ['nlpworkshop@gmail.com', 'support@python.org']
URLs:     ['https://www.nlpworkshop.com', 'www.python.org', 'https://github.com/NLPWorkshop']
Mobiles:  ['+91-9876543210', '9123456789']
Hashtags: ['#NLP', '#Python', '#MachineLearning']
Mentions: ['@NLPWorkshop', '@PythonLearner']
```

## Notes for Submission

- Each concept (stemming, lemmatization, regex) is isolated into its own clearly labeled section with a markdown explanation followed by code cells, so it can be read top to bottom as a lab report.
- If your lab requires a fresh run, use **Restart & Run All** before submitting/exporting to PDF so the outputs match the code exactly.
