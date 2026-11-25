# Abstractive Summarization with mT5 and mBART on XL-Sum (Hindi)

## Objective
Implement and evaluate abstractive text summarization using two multilingual Transformer sequence-to-sequence models and compare them using automatic metrics:

1. **mT5** (Multilingual Text-to-Text Transfer Transformer), checkpoint `google/mt5-small`
2. **mBART** (Multilingual Bidirectional and Auto-Regressive Transformer), checkpoint `facebook/mbart-large-50`

**Dataset:** [XL-Sum](https://huggingface.co/datasets/csebuetnlp/xlsum), BBC article and summary pairs in 45 languages. This experiment uses **Hindi**, and the language can be changed in one line.

**Metrics:** ROUGE-1, ROUGE-2, ROUGE-L and BERTScore (multilingual BERT, `bert-base-multilingual-cased`).

## Files
| File | Description |
|---|---|
| `mt5_vs_mbart_xlsum_summarization.ipynb` | Complete experiment notebook |
| `README.md` | This file |
| `results_mt5_vs_mbart.csv` | Metric table (created after running) |
| `generated_summaries.csv` | Reference, mT5 and mBART summaries for the test set (created after running) |

## Notebook Structure
| Part | Content |
|---|---|
| 1 | Install libraries, imports, configuration |
| 2 | Load XL-Sum and create train, validation and test subsets |
| 3 | Data exploration (examples, length statistics, histograms) |
| 4 | Metric functions: Unicode-safe ROUGE and BERTScore |
| 5 | Reusable pipeline: tokenization, fine-tuning, beam-search generation, scoring |
| 6 | Experiment A: fine-tune and evaluate mT5 |
| 7 | Experiment B: fine-tune and evaluate mBART |
| 8 | Comparison table, bar chart, loss curves, sample outputs |
| 9 | Conclusion |

## How to Run
1. Open the notebook in **Google Colab** (Runtime > Change runtime type > GPU) or any machine with a CUDA GPU (about 16 GB VRAM recommended for mBART).
2. Run all cells from top to bottom (Runtime > Run all).
3. Results are shown in Part 8 and saved as CSV files.

Local setup:
```bash
pip install "datasets<4.0" "transformers>=4.41" accelerate sentencepiece protobuf rouge_score bert_score matplotlib pandas jupyter
jupyter notebook mt5_vs_mbart_xlsum_summarization.ipynb
```

## Configuration (Part 1)
| Variable | Default | Meaning |
|---|---|---|
| `LANGUAGE` | `"hindi"` | XL-Sum language name (`nepali`, `bengali`, `telugu`, ...) |
| `MBART_LANG` | `"hi_IN"` | mBART-50 language code for the chosen language |
| `BERT_LANG` | `"hi"` | ISO code for BERTScore |
| `TRAIN_SIZE / VAL_SIZE / TEST_SIZE` | 3000 / 300 / 300 | Subset sizes for a fast run |
| `MAX_INPUT_LEN / MAX_TARGET_LEN` | 512 / 64 | Token limits for article and summary |
| `EPOCHS` | 3 | Fine-tuning epochs |
| `BATCH_SIZE x GRAD_ACCUM` | 4 x 4 | Effective batch size of 16 |
| `MT5_CHECKPOINT` | `google/mt5-small` | Can be replaced with `google/mt5-base` |

To switch language, change all three of `LANGUAGE`, `MBART_LANG` and `BERT_LANG`. Example for Nepali: `"nepali"`, `"ne_NP"`, `"ne"`. Bengali: `"bengali"`, `"bn_IN"`, `"bn"`. Telugu: `"telugu"`, `"te_IN"`, `"te"`.

## Method
- **Preprocessing:** whitespace normalization, tokenization with each model's own tokenizer (mBART uses source and target language codes).
- **Fine-tuning:** Hugging Face `Seq2SeqTrainer`. mT5 uses AdamW with learning rate 5e-4 and mBART uses Adafactor with learning rate 3e-5. mT5 is trained in bf16 or fp32 because fp16 can cause NaN losses.
- **Generation:** beam search (4 beams), `no_repeat_ngram_size=3`, maximum 64 new tokens.
- **Evaluation:** ROUGE-1/2/L F-measure with a custom Unicode tokenizer, and BERTScore precision, recall and F1.

## Important Notes
- The default ROUGE tokenizer discards non-Latin characters and would give wrong scores for Hindi. The notebook replaces it with a tokenizer that keeps Devanagari text (a sanity check in Part 4 should print 100 for identical strings).
- Scores are lower than published full-dataset numbers because only a subset is used. Increase `TRAIN_SIZE`, `EPOCHS`, or use `mt5-base` for better results.
- mT5-small often needs more training steps than mBART to produce fluent output, so low mT5 scores with small data are expected.
- XL-Sum is licensed CC BY-NC-SA 4.0 for non-commercial research use.
- If `load_dataset` fails because script-based datasets are unsupported, the notebook falls back automatically to the dataset's parquet branch.

## Citation
```
@inproceedings{hasan-etal-2021-xl,
    title = "{XL}-Sum: Large-Scale Multilingual Abstractive Summarization for 44 Languages",
    author = "Hasan, Tahmid and Bhattacharjee, Abhik and Islam, Md. Saiful and Mubasshir, Kazi and Li, Yuan-Fang and Kang, Yong-Bin and Rahman, M. Sohel and Shahriyar, Rifat",
    booktitle = "Findings of the Association for Computational Linguistics: ACL-IJCNLP 2021",
    year = "2021",
    pages = "4693--4703",
}
```
