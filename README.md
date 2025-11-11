# Flipkart iPhone 14 Review Analyzer

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Python Version](https://img.shields.io/badge/Python-3.8%2B-blue.svg)](https://www.python.org/downloads/)
[![Colab Compatible](https://img.shields.io/badge/Colab-Compatible-green.svg)](https://colab.research.google.com/)

## Overview

This project is a comprehensive **Natural Language Processing (NLP) pipeline** designed to scrape, preprocess, analyze, and summarize customer reviews for the iPhone 14 from Flipkart. It leverages traditional NLP techniques (no transformers) to perform sentiment analysis, topic modeling, semantic similarity, and automated summarization. The pipeline uncovers key insights such as user sentiments on battery life, camera quality, performance, display, and design, while generating data-driven Q&A for common queries.

### Key Objectives
- **Scrape** real-time reviews using Scrapy and Playwright for dynamic content.
- **Preprocess** multilingual reviews (with translation support) for clean, tokenized text.
- **Analyze** syntactically (POS tagging, NER), semantically (Word2Vec, TF-IDF), and sentiment-wise (VADER + LSTM).
- **Summarize** via clustering to highlight representative feedback and simulate Q&A.

The project processes ~1,000 reviews into actionable insights, saving results as JSON files for easy integration into reports or dashboards.

## Features
- **End-to-End Scraping**: Handles JavaScript-rendered pages with Playwright integration in Scrapy.
- **Multilingual Support**: Translates non-English reviews using Google Translate.
- **Advanced NLP**:
  - POS tagging and Named Entity Recognition (NER) for syntactic insights.
  - Bag-of-Words (BoW), TF-IDF, and Latent Semantic Analysis (LSA) for topic extraction.
  - Word2Vec for semantic similarities (e.g., "camera" → "quality", "photos").
  - Hybrid sentiment: Lexicon-based (VADER) + Deep Learning (LSTM).
- **Summarization & QA**: K-Means clustering on TF-IDF for representative reviews; rule-based Q&A grounded in analysis metrics.
- **Scalable & Reproducible**: Runs in Google Colab with GPU support; outputs structured JSON for downstream use.
- **Ethical Scraping**: Includes delays, user-agent rotation, and duplicate removal to respect site policies.

## Prerequisites
- Python 3.8+ (tested on 3.12).
- Google Colab (recommended for GPU acceleration during LSTM training) or local Jupyter environment.
- Basic familiarity with Jupyter notebooks.

## Installation

1. **Clone or Download the Repository**:
   ```bash
   git clone <your-repo-url>
   cd flipkart-iphone14-review-analyzer
   ```

2. **Set Up Environment** (in Colab or locally):
   - Open `main_analysis.ipynb` in Jupyter/Colab.
   - The notebook auto-installs dependencies via `!pip` commands. Run the first cell to install:
     ```
     !pip install scrapy scrapy-playwright playwright indic-nlp-library langdetect googletrans==3.1.0a0 langid nltk scikit-learn gensim vaderSentiment keras tensorflow spacy
     !playwright install chromium
     !python -m spacy download en_core_web_sm
     !python -m nltk.downloader punkt punkt_tab stopwords wordnet averaged_perceptron_tagger_eng maxent_ne_chunker words maxent_ne_chunker_tab
     ```

3. **API Keys (Optional)**:
   - No external APIs required beyond free tiers (e.g., Google Translate via `googletrans`).

## Usage

### Running the Pipeline
1. **Launch in Colab**:
   - Upload or open `main_analysis.ipynb`.
   - Execute cells sequentially (Ctrl+F9 in Colab).
   - The pipeline runs in ~10-15 minutes (scraping: 5-7 min; analysis: 3-5 min; summarization: 2 min).

2. **Step-by-Step Execution**:
   - **Cell 1: Setup & Scraping** – Installs libs, scrapes ~1,000 reviews, saves to `reviews.json`.
   - **Cell 2: Preprocessing & Core Analysis** – Cleans data, runs POS/NER/sentiment/LSA/Word2Vec, saves to `reviews2.json` and `preprocessed_reviews_results.json`.
   - **Cell 3: Summarization & QA** – Clusters reviews, extracts representatives, generates Q&A, saves to `final_summary.json`.

3. **Customization**:
   - **Product URL**: Edit the Scrapy spider's `start_urls` in Cell 1 (default: iPhone 14 on Flipkart).
   - **Review Limit**: Set `CRAWL_MAX_REVS = 1000` in the spider.
   - **Clusters**: Adjust `n_clusters=5` in Cell 3 for more/fewer summary groups.
   - **Q&A**: Modify `QA_PAIRS` list in Cell 3 to add custom questions.

### Example Output
- **Console Insights**:
  ```
  Overall VADER Sentiment: 0.61 (Moderately Positive)
  Overall LSTM Sentiment: 0.878 (Strongly Positive)
  Topic 1: ['awesome', 'iphone', 'quality', 'performance', 'nice', 'best', 'product', 'camera', 'phone', 'good']
  Similar to 'camera': [('battery', 0.9959), ('iphone', 0.9957), ('good', 0.9956), ...]
  ```
- **Sample Q&A**:
  ```
  Q: Is the battery life good for daily use?
  A: Yes, based on 68% positive mentions... Representative: 'battery backup amazing... effective according daily usage'
  ```

## Project Structure
```
flipkart-iphone14-review-analyzer/
├── main_analysis.ipynb          # Core Jupyter notebook (all phases)
├── reviews.json                 # Raw scraped reviews
├── reviews2.json                # Preprocessed reviews (tokenized, lemmatized)
├── preprocessed_reviews_results.json  # Analysis metrics (POS, sentiments, topics)
└── final_summary.json           # Summaries, clusters, Q&A
```

- **Phases in Notebook**:
  1. **Scraping**: Scrapy spider for Flipkart reviews.
  2. **Preprocessing**: Cleaning, tokenization, lemmatization (NLTK).
  3. **Analysis**: Syntactic (POS/NER), Semantic (TF-IDF/LSA/Word2Vec), Sentiment (VADER/LSTM).
  4. **Summarization**: K-Means clustering, representative extraction, data-driven Q&A.

## Outputs & Insights
- **JSON Files**: Structured for easy parsing (e.g., load with `json.load()`).
- **Key Metrics**:
  - **Sentiment**: VADER (lexicon) + LSTM (trained on review corpus).
  - **Topics**: 5 LSA-derived themes (e.g., camera, battery).
  - **Similarities**: Word2Vec associations for features like "design" → "premium".
  - **Clusters**: 5 groups with representatives (e.g., Cluster 1: "amazing phone iphone" – 61% of reviews).
- **Insights from iPhone 14 Reviews**:
  - 82% praise camera (low-light, cinematic modes).
  - Battery: Strong for daily use (0.62 VADER score).
  - Performance: "Mind-blowing" speed, minor heating issues (12%).
  - Overall: 87% positive, value-for-money at premium price.

## Dependencies
| Library | Purpose | Version |
|---------|---------|---------|
| `scrapy` | Web scraping | 2.13.3 |
| `playwright` | JS rendering | 1.55.0 |
| `nltk` | Tokenization, POS, NER | Latest |
| `scikit-learn` | TF-IDF, LSA, Clustering | Latest |
| `gensim` | Word2Vec | Latest |
| `vaderSentiment` | Lexicon sentiment | Latest |
| `tensorflow/keras` | LSTM model | Latest |
| `spacy` | (Optional) NER backup | en_core_web_sm |
| `googletrans` | Translation | 3.1.0a0 |

Full list auto-installed in notebook.

## Limitations & Improvements
- **Scraping**: Rate-limited; may need proxies for large-scale.
- **Sentiment**: LSTM is binary/simple; extend to multi-class.
- **Multilingual**: Relies on Google Translate; accuracy ~85% for Indic languages.
- **Future**: Integrate transformers (BERT) for advanced QA; add visualization (e.g., word clouds via Matplotlib).

## Contributing
1. Fork the repo.
2. Create a feature branch (`git checkout -b feature/AmazingFeature`).
3. Commit changes (`git commit -m 'Add some AmazingFeature'`).
4. Push to branch (`git push origin feature/AmazingFeature`).
5. Open a Pull Request.

## Acknowledgments
- Built with open-source tools: Scrapy, NLTK, scikit-learn, TensorFlow.
- Inspired by e-commerce sentiment analysis research.
- Thanks to Flipkart for public review data (used ethically).

---
