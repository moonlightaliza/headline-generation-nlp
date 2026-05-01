# Automatic Headline Generation (NLP Project)

## Description
This is a collaborative project for our 6th semester Fundamentals of NLP course.  
The goal is to automatically generate concise, relevant, and informative headlines from news articles using Natural Language Processing techniques.

## Team Members
- Aliza Zia  
- Um E Kulsoom  

## Objectives
- Generate high-quality headlines from long news articles  
- Compare traditional and transformer-based models  
- Evaluate performance using standard NLP metrics  

## Dataset
- [CNN/DailyMail](https://www.kaggle.com/datasets/gowrishankarp/newspaper-text-summarization-cnn-dailymail)
- [News Summary](https://www.kaggle.com/datasets/sunnysai12345/news-summary) (for validation)

## Models
- Seq2Seq (LSTM + Attention) – Baseline  
- BART (Fine-tuned Transformer)  
- T5 (Text-to-Text Transformer)  

## How to Run
1. Open `nlp_project.ipynb` in Google Colab
2. Mount Google Drive
3. Run All cells sequentially

## Requirements
- tensorflow
- transformers
- kagglehub
- rouge_score
- torch

## Evaluation Metrics
- ROUGE (ROUGE-1, ROUGE-2, ROUGE-L)  
- BLEU Score  
- BERTScore  

## Project Status
✅ Complete
