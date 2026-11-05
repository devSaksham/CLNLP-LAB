# Experiment 5: Subword Tokenization, POS Tagging and Word2Vec

| Section | Topic | Library |
|---|---|---|
| 5.1 | BPE tokenization: GPT-2's pretrained tokenizer, and one trained from scratch | `transformers`, `tokenizers` |
| 5.2 | SentencePiece: pretrained, and trained from scratch | `transformers`, `sentencepiece` |
| 5.3 | POS tagging of a single sentence | spaCy, NLTK |
| 5.4 | POS tag frequencies over the input file | spaCy |
| 5.5 | Word2Vec, CBOW | gensim |
| 5.6 | Word2Vec, Skip-Gram | gensim |
| 5.7 | Cosine similarity between word pairs | gensim |

## Files

| File | Description |
|---|---|
| `Experiment_5_Tokenization_POS_Tagging.ipynb` | The notebook, with outputs |
| `input_sub_word_data.txt` | Input text (the notebook downloads it with `gdown` if it's missing) |
| `sp_training_corpus.txt`, `sp_scratch_model.model`, `sp_scratch_model.vocab` | SentencePiece corpus and model written by 5.2(b) |
| `Experiment-5.docx` | The lab sheet |

## Run

The first cell installs everything:

```bash
pip install tokenizers transformers sentencepiece spacy nltk gensim pandas gdown
python -m spacy download en_core_web_sm
```

Then run the cells in order in Jupyter or Colab. Word2Vec uses a fixed seed and one worker, so the vectors are reproducible.
