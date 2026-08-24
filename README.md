# Cyberbullying Detection System

A machine learning system that classifies text as cyberbullying or non-bullying content, built and evaluated on a labeled dataset of ~18,000 real social media comments.

## Problem Statement

Online platforms generate more user text than any moderation team can manually review. This project trains and compares multiple text-classification models to automatically flag bullying/harassing content, with the goal of a lightweight, deployable detector rather than a black-box classifier.

## Dataset

- **~18,000 labeled comments** (`dataset.csv`) — real user-generated text labeled as bullying (`-1`) or non-bullying
- Raw, informal social-media text: slurs, slang, and noise are present by design, since that's what a real detector has to handle

## Approach

1. **EDA** — class balance check via pie chart to confirm whether the dataset is balanced or skewed
2. **Text cleaning** — regex-based preprocessing (`re`) to strip usernames/mentions, collapse repeated characters, and remove noise typical of Twitter-style text
3. **Feature extraction** — TF-IDF vectorization (chosen over bag-of-words for speed and lower memory footprint) with a custom stopword list (`stopwords.txt`)
4. **Train/test split** — 70/30
5. **Model training** — trained and pickled 7 classifiers for comparison: LinearSVC, Logistic Regression, Multinomial Naive Bayes, Decision Tree, AdaBoost, Bagging Classifier, and SGD Classifier
6. **Hyperparameter tuning** — GridSearchCV on LinearSVC's `C` parameter (5-fold CV)
7. **Final model** — tuned **LinearSVC** (`C=1.2`) selected as the production model:

   | Metric | Score |
   |---|---|
   | Accuracy | 96.4% |
   | F1 Score | 97.2% |
   | Precision | 97.3% |
   | Recall | 97.2% |

## Tech Stack

- **Language:** Python
- **ML/NLP:** scikit-learn, XGBoost, TF-IDF vectorization
- **Data:** pandas, NumPy
- **Visualization:** Matplotlib, Seaborn
- **Web:** Flask
- **Environment:** Jupyter Notebook

## Project Structure

```
├── Cyber Bulling Detection Using Python (1).ipynb   # Full training & evaluation notebook
├── dataset.csv                                       # Labeled comment dataset
├── stopwords.txt                                     # Custom stopword list used in TF-IDF
├── tfidfvectoizer.pkl                                # Fitted TF-IDF vocabulary
├── LinearSVCTuned.pkl                                # Final production model
├── LogisticRegression.pkl, MultinomialNB.pkl,
│   DecisionTreeClassifier.pkl, SGDClassifier.pkl,
│   LinearSVC.pkl, BaggingClassifier.pkl,
│   AdaBoostClassifier.pkl                            # Comparison models from the evaluation pipeline
└── app.py                                            # Flask app serving the model for live predictions
```

## Running Locally

```bash
pip install flask scikit-learn
python app.py
```

Then open `http://127.0.0.1:5000` and submit text to get a live prediction.

## Current Status

✅ Data cleaning, TF-IDF pipeline, and model comparison complete
✅ LinearSVC tuned and selected as final model
✅ Basic Flask app for live single-input predictions

## Future Scope

This repository covers the core detection model. A more advanced, full-stack version — **CyberShield** — is in development separately, extending this into a complete cyberbullying prevention platform:
- Role-based authentication with parent-child account linking and automatic parent alerts
- Additional AI agents: text rephrasing, URL safety scanning (Google Safe Browsing), and spam/scam detection
- Dashboard with Chart.js analytics and a dark-themed UI
- Deployment to a public URL (Render)

## Author

Dev — B.Tech CSE (AI/ML), K.R. Mangalam University
