# AutoJudge
# Codeforces Problem Difficulty Predictor

## Project Objective
The goal of this project is to predict the difficulty of Codeforces programming problems using the text content of the problem. Given the problem’s title, description, input format, and output format, the system predicts:

1. Difficulty class: Easy, Medium, or Hard  
2. Numeric difficulty score: a rating approximating Codeforces’ own problem rating  

This can help learners and competitive programmers estimate problem difficulty before attempting a problem and can be used in automated learning systems or contest preparation tools.

---

## Dataset Description
We used the **Codeforces dataset** available via Hugging Face (`open-r1/codeforces`). The dataset contains:

- `title`: Problem title  
- `description`: Problem statement  
- `input_format` / `output_format`: Input and output specifications  
- `rating`: Numeric difficulty score assigned by Codeforces  

We filtered out problems with missing or empty descriptions and missing ratings. The final dataset contains only entries with valid text and labels.

---

## Preprocessing Steps
To prepare the data for modeling:

1. Combined text columns (`title`, `description`, `input_format`, `output_format`) into a single combined text field.  
2. Cleaned text:
   - Converted to lowercase  
   - Removed extra spaces and newlines  
   - Retained only letters, numbers, and key math symbols (`+-*/=^<>%`)  
3. Dropped rows with missing labels and reset the index.

---

## Feature Engineering

We used a combination of text-based and numeric features:

### 1. TF-IDF
- Converted combined text into **TF-IDF vectors** (unigrams and bigrams) with a maximum of 5000 features.  
- Captures the importance of words/phrases in distinguishing problems.

### 2. Extra Numeric Features
- Text length: Total number of characters in the combined text  
- Word count: Total number of words  
- Math symbols: Count of `+-*/=^<>%` symbols

### 3. Keyword Frequency Features
- Counted occurrences of algorithmic keywords such as:  
  `dp`, `greedy`, `graph`, `tree`, `segment tree`, `bitmask`, `max flow`, etc.  
- These features explicitly encode algorithmic complexity cues that may not be fully captured by TF-IDF vectors.

---

## Model Choice & Reasoning

### Classification
- **Model:** Logistic Regression  
- **Reasoning:**  
  - Handles high-dimensional sparse features efficiently (TF-IDF vectors)  
  - Provides a simple and interpretable model for predicting difficulty class (Easy/Medium/Hard)  
  - Achieved **~60% accuracy** on the dataset

### Regression
- **Model:** Ridge Regression  
- **Reasoning:**  
  - Predicts numeric difficulty scores  
  - Ridge regularization prevents overfitting due to correlated numeric and keyword features  
  - Applied logarithmic transformation to target scores to stabilize variance and handle skewed distribution  
  - After prediction, inverse log transform produces final predicted scores

---

## Evaluation Results

### Classification
- **Accuracy:** 0.603 (~60%)  

### Regression
- **Mean Absolute Error (MAE):** 605.89  
- **Root Mean Squared Error (RMSE):** 737.48  
- Limitations:  
  - Exact difficulty prediction from text alone is challenging due to the subjective nature of problem ratings  
  - Keyword features improved interpretability but did not significantly improve regression accuracy  
  - Short or uncommon problem descriptions sometimes lead to underestimated scores

---

## Challenges / Limitations
- Skewed difficulty ratings make numeric score prediction challenging.  
- Regression accuracy is limited by **training data sparsity** for Hard problems.  
- Text alone may not fully capture problem complexity; test cases, constraints, or additional metadata could improve predictions.  

---
