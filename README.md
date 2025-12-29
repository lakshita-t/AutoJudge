# AutoJudge
Your README should include:
•	Project objective
•	Dataset description
•	Preprocessing steps
•	Feature engineering details
o	TF-IDF
o	Text length / math symbols / keywords
•	Model choice & reasoning
o	Logistic Regression for classification
o	Ridge Regression for regression
•	Evaluation results
o	Classification: Accuracy, Confusion Matrix
o	Regression: MAE, RMSE, maybe median error
•	Challenges / Limitations
o	Why MAE is high (noisy ratings)
o	Why adding keywords didn’t improve regression
•	How to run the Streamlit app

Since difficulty ratings are highly skewed, a logarithmic
transformation of the target variable was applied to stabilize variance
and improve regression performance
Predicting exact difficulty scores from text alone is inherently
challenging due to the subjective nature of problem ratings. Despite
this, our regression model achieved an MAE of approximately 600, which
is significantly better than random baselines.
Keyword frequency features were used to explicitly encode algorithmic
complexity cues that may not be fully captured by TF-IDF vectors.
Optional but impressive
•	Error analysis plots
o	Histogram of regression errors
o	Confusion matrix heatmap
•	Feature importance visualization
o	For keywords and extra numeric features
•	Future improvements
o	Use BERT / CodeBERT embeddings
o	Predict rating buckets instead of exact scores
o	Multi-modal features (test cases, constraints)
