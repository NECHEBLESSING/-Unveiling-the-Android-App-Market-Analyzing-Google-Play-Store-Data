# Unveiling-the-Android-App-Market-Analyzing-Google-Play-Store-Data
# Overview
The Google Playstore is a vast marketplace for mobile apps with millions of apps available for download. This report aims to provide insights into the Playstore dataset, exploring trends, patterns, and correlations between various apps charactristics.
# Data Source
- kaggle
# Data Processing
The dataset will be cleaned and processed to remove missing values,duplicates, and
irrelevant data. The processing steps includes;
- Handling missing values
- Data Normalization
- Feature scaling
### Handling missing values
*Removing NA Values and Data Cleaning*
app_df=app_df.dropna()
user_reviews_df=user_reviews_df.dropna()

