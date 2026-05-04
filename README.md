# 📰 Automatic Headline Generation Using NLP

> Comparing Seq2Seq BiLSTM · BART · T5 + Hybrid Attention for abstractive news headline generation

**Authors:** Aliza Zia · Um E Kulsoom  
**Course:** Fundamentals of NLP — 6th Semester  
**Submission:** 03 May 2026

---

## 📋 Overview

This project investigates three NLP architectures of increasing sophistication for the task of **automatic news headline generation** — producing a short, informative title from a longer news article.

| Property | Seq2Seq BiLSTM | BART | T5 + Hybrid Attention |
|---|---|---|---|
| Base | BiLSTM + Bahdanau Attn | `facebook/bart-base` | `t5-small` + custom attn |
| Parameters | 27.9M | 139.4M | 62.6M |
| Encoder | Bidirectional LSTM (128u) | Transformer encoder | T5 encoder |
| Decoder | LSTM (256 units) | Transformer decoder | T5 decoder + HybridAttn |
| Attention | Bahdanau | Multi-head self-attention | Local (window=4) + Global |
| Training | 30 epochs | 3 epochs, AdamW lr=5e-5 | 10 epochs, batch=8 |
| Vocab / Tokens | 20,000 / max 400 | 50,265 / max 512 | 32,128 / max 512 |

---

## 📦 Datasets

### Primary — CNN / DailyMail
The CNN/DailyMail corpus (~300K article–headline pairs). Due to computational constraints, **5,000 samples** were selected (random_state=42) and split as follows:

| Split | Samples | Proportion |
|---|---|---|
| Train | 4,000 | 80% |
| Validation | 500 | 10% |
| Test | 500 | 10% |

### Cross-Validation — News Summary (Kaggle)
[`sunnysai12345/news-summary`](https://www.kaggle.com/datasets/sunnysai12345/news-summary) — used **only for out-of-distribution inference**, not training. Serves as a reliable probe of generalization to unseen news distributions.

---

## 📊 Results

### Primary Dataset (CNN / DailyMail)

| Metric | BiLSTM | BART | T5+Hybrid | Best |
|---|---|---|---|---|
| ROUGE-1 | 0.0501 | **0.3689** | 0.3369 | BART |
| ROUGE-2 | 0.0000 | **0.1432** | 0.1224 | BART |
| ROUGE-L | 0.0453 | **0.2471** | 0.2242 | BART |
| Params | 27.9M | 139.4M | **62.6M** | T5+Hybrid |

### Cross-Validation (News Summary Dataset)

| Metric | BiLSTM | BART | T5+Hybrid | Best |
|---|---|---|---|---|
| ROUGE-1 | 0.0321 | 0.2334 | **0.2650** | T5+Hybrid |
| ROUGE-2 | 0.0000 | 0.0868 | **0.0991** | T5+Hybrid |
| ROUGE-L | 0.0289 | 0.1934 | **0.2220** | T5+Hybrid |

### Generalization Gap (Primary − Cross-Val, smaller = better)

| Metric | BiLSTM Gap | BART Gap | T5+Hybrid Gap | Best Generalizer |
|---|---|---|---|---|
| ROUGE-1 | +0.0180 | +0.1355 | +0.0719 | BiLSTM* |
| ROUGE-2 | +0.0000 | +0.0564 | +0.0232 | BiLSTM* |
| ROUGE-L | +0.0164 | +0.0537 | **+0.0022** | T5+Hybrid |

*BiLSTM's small gap reflects near-zero scores on both datasets, not genuine generalization.*

---

## 🔍 Key Findings

- **BART** achieves the highest scores on CNN/DailyMail (R-1: 0.3689) despite only 3 fine-tuning epochs, thanks to its large pre-trained vocabulary and self-supervised pre-training on news corpora.
- **T5+Hybrid** outperforms BART on the cross-validation set (R-1: 0.2650 vs 0.2334), reversing the ranking and demonstrating stronger generalization to unseen distributions — with less than half the parameters of BART.
- **BiLSTM** suffers from severe repetition (e.g., outputs like *"the the the the the…"*), a known failure mode of LSTM decoders without coverage mechanisms. ROUGE-2 is exactly 0.0000.
- Transformer models outperform BiLSTM by **~7× on ROUGE-1**, confirming pre-trained transformers are significantly better suited to abstractive headline generation.

---

## 💬 Qualitative Example

| Model | Generated Headline |
|---|---|
| **Reference** | Bob Slade, 65, threatened with suspension for high-fiving school children by council bosses. |
| BiLSTM | *the the the the the the the the the the the* |
| BART | Bob Slade, 65, was threatened with suspension for high-fiving school children by council bosses. |
| T5+Hybrid | Bob Slade was threatened with suspension for high-fiving children at Manadon Vale Primary School. |

---


## ⚙️ Setup & Installation

```bash
git clone https://github.com/moonlightaliza/headline-generation-nlp.git
cd headline-generation-nlp
pip install -r requirements.txt
```

**Requirements include:**
- `transformers>=4.30`
- `datasets`
- `rouge-score`
- `torch`
- `sentencepiece`

---


## ⚠️ Limitations

- Only 5,000 of 300,000 available CNN/DailyMail samples were used due to computational constraints.
- BART was limited to 3 fine-tuning epochs due to GPU memory limits; more epochs would likely improve scores.
- ROUGE measures n-gram overlap but does not capture semantic fidelity. BLEU and BERTScore evaluation remains pending.
- The BiLSTM decoder lacks a coverage mechanism, leading to degenerate repetitive outputs.

---

## 🔭 Future Work

- Train on the full CNN/DailyMail corpus (300K samples)
- Increase BART fine-tuning to 10+ epochs with gradient accumulation
- Add BERTScore and BLEU for semantic evaluation
- Explore PEGASUS (pre-trained specifically for summarization)
- Investigate coverage mechanisms for BiLSTM to suppress repetition
- Expand cross-dataset validation to XSum and Gigaword
- Deploy the best model as a real-time web API

---

## 📄 Reference

This project's hybrid attention design is informed by:

> Wan, W.; Zhang, C.; Huang, L. *Efficient Headline Generation with Hybrid Attention for Long Texts.* Electronics 2024, 13, 3558. https://doi.org/10.3390/electronics13173558

---

## 🤝 Authors

| Name | GitHub |
|---|---|
| Aliza Zia | [@moonlightaliza](https://github.com/moonlightaliza) |
| Um E Kulsoom | [@KayTheCoder-101](https://github.com/KayTheCoder-101)|

---

*Submitted for Fundamentals of NLP, 6th Semester — May 2026*
