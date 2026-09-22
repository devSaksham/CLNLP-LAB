# Experiment 5 — Subword Tokenization & POS Tagging

This project implements and compares subword tokenization methods (BPE and
SentencePiece) and Part-of-Speech tagging methods (spaCy and NLTK) in a
single, clean Jupyter notebook: **`Experiment_5_Tokenization_POS_Tagging.ipynb`**.

## Contents

| Section | Task | Approach |
|---|---|---|
| 5.1 (a) | BPE subword tokenization | Pretrained — GPT-2 tokenizer (`transformers`) |
| 5.1 (b) | BPE subword tokenization | From scratch — custom BPE trainer/encoder |
| 5.2 (a) | SentencePiece tokenization | Pretrained — T5 tokenizer (`transformers`) |
| 5.2 (b) | SentencePiece tokenization | From scratch — trained with the `sentencepiece` library |
| 5.3 (a) | POS tagging on a fixed sentence | spaCy |
| 5.3 (b) | POS tagging on a fixed sentence | NLTK |
| 5.4 | POS tagging + frequency count on the 5.1 input data | spaCy |

Each part prints the input sentence and displays a table of
**token → token ID** (5.1, 5.2) or **token → POS tag → description**
(5.3, 5.4, plus frequency for 5.4).

## Input Data

The notebook expects a plain-text file named **`input_sub_word_data.txt`**
in the same folder as the notebook.

Source file: [Google Drive link](https://drive.google.com/file/d/1eHA0Q9ju-08n_hByCK03GqWAX9HQvAKd/view?usp=sharing)

**Setup:**
1. Open the Drive link → File → Download.
2. Rename the downloaded file to `input_sub_word_data.txt` if needed.
3. Place it in the same directory as the `.ipynb` file (or upload it to
   `/content/` if you are running on Google Colab).

> If the file isn't found, the notebook automatically falls back to a
> short built-in sample paragraph so every cell still runs end-to-end —
> but for a real submission, make sure the actual file is present.

## Requirements

- Python 3.9+
- Packages (installed by the first cell of the notebook):
  - `transformers`
  - `sentencepiece`
  - `spacy` (+ the `en_core_web_sm` model)
  - `nltk` (+ `punkt` and `averaged_perceptron_tagger` resources)
  - `pandas`

You can also install everything up front:

```bash
pip install transformers sentencepiece spacy nltk pandas
python -m spacy download en_core_web_sm
```

An internet connection is required the first time you run the notebook,
since it downloads:
- the pretrained GPT-2 tokenizer (5.1a) and T5 tokenizer (5.2a) from
  Hugging Face,
- the spaCy `en_core_web_sm` model,
- the NLTK `punkt` / `averaged_perceptron_tagger` data.

## How to Run

1. Install the requirements (see above), or just run the first notebook
   cell, which does `pip install` + `spacy download` for you.
2. Place `input_sub_word_data.txt` next to the notebook (see **Input
   Data** above).
3. Open the notebook and run all cells top to bottom
   (`Kernel → Restart & Run All` in Jupyter, or `Runtime → Run all` in
   Colab).

## Notebook Structure / Design Notes

- **Section 0 — Setup**: installs dependencies and imports everything
  used later, so no imports are hidden inside later cells.
- **Section 1 — Load Input Data**: reads `input_sub_word_data.txt`,
  previews it, and extracts one representative sentence
  (`sample_sentence`) used to demonstrate the 5.1 / 5.2 tokenizers.
- **5.1 (a) / 5.2 (a) — Pretrained**: use Hugging Face `transformers`
  tokenizers, which internally implement BPE (GPT-2) and SentencePiece
  (T5). `.tokenize()` gives the subword strings, `.convert_tokens_to_ids()`
  gives their IDs.
- **5.1 (b) — BPE from scratch**: implements the original
  Sennrich et al. (2016) algorithm — build a character-level vocabulary,
  repeatedly merge the most frequent adjacent symbol pair, then apply the
  learned merges to tokenize new text. Token IDs are assigned by
  enumerating the unique subwords produced.
- **5.2 (b) — SentencePiece from scratch**: trains a brand-new
  SentencePiece **unigram** model directly on the corpus using
  `sentencepiece.SentencePieceTrainer.train()`, then loads it to tokenize
  the sample sentence. `vocab_size` auto-adjusts downward if the corpus is
  too small for the requested size.
- **5.3 — POS tagging**: run on the fixed sentence *"The young student is
  reading an interesting book in the library."* spaCy uses its own
  fine-grained tags (`token.tag_` + `spacy.explain`); NLTK uses Penn
  Treebank tags (`nltk.pos_tag` + a lookup table of tag descriptions).
- **5.4 — POS tagging with frequency**: reuses the full 5.1 input data,
  tags every token with spaCy, and counts how often each
  (token, POS tag) pair occurs, sorted by frequency.

## Output

All results are displayed as `pandas` DataFrames directly in the
notebook (no separate output files are produced). Each cell's output can
be copied into a report as-is.

## Troubleshooting

- **`OSError: en_core_web_sm not found`** → run
  `python -m spacy download en_core_web_sm` and restart the kernel.
- **NLTK `LookupError`** → re-run the setup cell; it downloads `punkt`
  and `averaged_perceptron_tagger` automatically.
- **SentencePiece training error about vocab size** → the notebook
  already retries with a smaller `vocab_size` for small corpora; if it
  still fails, lower the initial `vocab_size` value in the 5.2(b) cell.
- **No internet access** → 5.1(a) and 5.2(a) need to download pretrained
  models the first time; all other sections work fully offline once
  spaCy/NLTK data is cached locally.
