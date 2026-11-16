# CLNLP Lab

Lab work for my university NLP course: five experiments that go from pandas EDA to text preprocessing, NER, TF-IDF, subword tokenizers and word embeddings. Each folder has its own notebook (saved with outputs), input data and README.

| Lab | Topic | Main tools |
|---|---|---|
| [Lab 1](CNLP_LAB1) | Exploratory analysis of an employee dataset: grouping, filtering, bar/pie/histogram/scatter plots | pandas, matplotlib |
| [Lab 2](CNLP_LAB2) | Text preprocessing: lowercasing, punctuation and number removal, word/sentence tokenization, stop-word removal | Python, NLTK, spaCy |
| [Lab 3](CNLP_LAB3) | Porter stemming, POS-aware lemmatization, and regex information extraction | NLTK, `re` |
| [Lab 4](CNLP_LAB4) | Term frequency and NER with spaCy, term frequency without any toolkit, and TF-IDF from scratch | spaCy, pandas |
| [Lab 5](CNLP_LAB5) | BPE and SentencePiece tokenizers (pretrained and trained from scratch), POS tagging, Word2Vec CBOW vs Skip-Gram | transformers, tokenizers, sentencepiece, gensim |

## Running a lab

```bash
git clone https://github.com/devSaksham/CLNLP-LAB.git
cd CLNLP-LAB/CNLP_LAB4
pip install jupyter pandas matplotlib nltk spacy
python -m spacy download en_core_web_sm
jupyter notebook
```

Run each notebook from inside its own folder, since the input files are read by relative path. Lab 5 needs a few more packages; see its README.

## License

MIT
