# Unveiling-the-Android-App-Market-Analyzing-Google-Play-Store-Data
# Overview
The Google Playstore is a vast marketplace for mobile apps with millions of apps available for download. This report aims to provide insights into the Playstore dataset, exploring trends, patterns, and correlations between various apps charactristics.
# Data Source
- kaggle [DATASET](https://www.kaggle.com/datasets/utshabkumarghosh/android-app-market-on-google-play)
# Data Processing
The dataset will be cleaned and processed to remove missing values,duplicates, and
irrelevant data. The processing steps includes;
- Handling missing values
- Data Normalization
- Feature scaling
  
### Handling missing values
*Removing NA Values and Data Cleaning*

```
app_df=app_df.dropna()
user_reviews_df=user_reviews_df.dropna()
```

*Clean Data*
```
data_clean=app_df.drop(columns=['Unnamed: 0'])
```
*Handle 'Installs' Column*
```
data_clean['Installs']=data_clean['Installs'].str.replace('[+,]','',regex=True).astype(int)
```
*Handle 'Size' column*
```
data_clean['Size']= data_clean['Size'].replace('Varies with device',
pd.NA)
data_clean['Size']= data_clean['Size'].apply(lambda x:
float(str(x).replace('M','').replace('k','')) if isinstance(x,str)else pd.NA)
```
*Handle 'Price' column*
```
data_clean['Price']=data_clean['Price'].str.replace('$','',regex=False).astype(float)
```
*Fill Missing 'Rating' with Mean and drop rows with missing 'Type',
'Current ver','Andriod ver'*
```
data_clean['Rating'].fillna(data_clean['Rating'].mean(), inplace=True)
data_clean.dropna(subset=['Type','Current Ver', 'Android Ver'],
inplace=True)
```
```
data_clean['Last Updated']=pd.to_datetime(data_clean['Last Updated'])
```
```
data_clean['Rating'].fillna(data_clean['Rating'].mean(),
inplace=True)
```
```
desc_stats=data_clean.describe()
print(desc_stats)
```
![U-A ](https://github.com/NECHEBLESSING/-Unveiling-the-Android-App-Market-Analyzing-Google-Play-Store-Data/blob/main/U-A%203.PNG)

# EXPLORATORY DATA ANALYSIS (EDA)
EDA will be performed to understand the distribution of app characteristics, including rating,
installs, and content rating. These analysis will reveal:
- Distribution of app ratings
- Top categories by number of apps
- Relationship between app rating and installs

### DISTRIBUTION OF APP RATING AND APP INSTALL
The distribution of ratings and installs was analyzed to understand the trends and patterns. The
results showed:
- Most apps having a rating between 4 - 5
- Top categories by number of installs

*Distribution of app ratings*
```
plt.figure(figsize=(8,6))
sns.histplot(data_clean['Rating'], bins=20, kde=True, color='purple')
plt.title('Distribution of App Ratings')
plt.xlabel('Rating')
plt.ylabel('Frequency')
plt.show()
```
![U-A](https://github.com/NECHEBLESSING/-Unveiling-the-Android-App-Market-Analyzing-Google-Play-Store-Data/blob/main/U-A%204.PNG)

*Distribution of app ratings*
```
plt.figure(figsize=(8,6))
sns.histplot(data_clean['Installs'], bins=20, kde=True, color='purple')
plt.title('Distribution of App Installs')
plt.xlabel('Installs')
plt.ylabel('Frequency')
plt.show()
```
![U-A](https://github.com/NECHEBLESSING/-Unveiling-the-Android-App-Market-Analyzing-Google-Play-Store-Data/blob/main/U-A%205.PNG)

### Top categories by number of apps
*APP RATING VS CONTENT RATING*
The relationship between app rating and content rating was explored to understand how
content rating affects app rating. The analysis revealed:
- Correlation between app rating and content rating
- Content rating distribution by category

*App Rating by content Rating*
```
plt.figure(figsize=(10,6))
sns.boxplot(x='Content Rating', y='Rating', color='purple',data=data_clean)
plt.title('App Ratings by Content Rating')
plt.xticks(rotation=45)
plt.show()
```
![U-A](https://github.com/NECHEBLESSING/-Unveiling-the-Android-App-Market-Analyzing-Google-Play-Store-Data/blob/main/U-A%206.PNG)

###NUMBER OF APPS BY CATEGORY
The number of apps by category was analyzed to understand the most popular categories. The
results showed:
- Top categories by number of apps

*Numbers of app by category*
```
plt.figure(figsize=(12,6))
sns.countplot(y='Category', data=data_clean,order=data_clean['Category'].value_counts().index, palette= 'viridis')
plt.title('Number of Apps by Category')
plt.xlabel('Count')
plt.ylabel('Category')
plt.show()
```
![U-A ](https://github.com/NECHEBLESSING/-Unveiling-the-Android-App-Market-Analyzing-Google-Play-Store-Data/blob/main/U-A%207.PNG)

### CORRELATION ANALYSIS
Correlation analysis was performed to understand the relationships between various app
characteristics. The results showed:
- Correlation between Rating and Reviews
- Correlation between Installs and Prices
```
corr_matrix=data_clean[['Rating','Reviews','Installs','Price']].corr()
plt.figure(figsize=(8,6))
sns.heatmap(corr_matrix, annot=True, cmap='purples',linewidths=0.5)
plt.title('Correlation Matrix of Numerical Features')
plt.show()
```
![U-A](https://github.com/NECHEBLESSING/-Unveiling-the-Android-App-Market-Analyzing-Google-Play-Store-Data/blob/main/U-A%208.PNG)

### TOP 10 INSTALLED APPS, RATING AND REVIEWS
The top 10 install app,rating and reviews were analyzed to understand the characteristics of
popular apps. The results showed:
- Top Installed app
- Top Rating
- Top Reviews
*Top 10 by Installs*
```
top_installs=data_clean[['App','Installs']].sort_values(by='Installs',ascending=False).head(10)
print(top_installs)
```
![U-A](https://github.com/NECHEBLESSING/-Unveiling-the-Android-App-Market-Analyzing-Google-Play-Store-Data/blob/main/U-A%209.PNG)

*Top 10 by Rating*
```
top_rating=data_clean[['App','Rating']].sort_values(by='Rating',ascend
ing=False).head(10)
print(top_rating)
```
![U-A](https://github.com/NECHEBLESSING/-Unveiling-the-Android-App-Market-Analyzing-Google-Play-Store-Data/blob/main/U-A%2010.PNG)

*Top 10 by Reviews*
```
top_reviews=data_clean[['App','Reviews']].sort_values(by='Reviews',asc
ending=False).head(10)
print(top_reviews)
```
![U-A](https://github.com/NECHEBLESSING/-Unveiling-the-Android-App-Market-Analyzing-Google-Play-Store-Data/blob/main/U-A%2011.PNG)

### PRICE OF PAID APPS AND FREE APPS
The price of paid apps and free apps was analyzed to understand the pricing trends. The results
showed:
- Distribution of free and paid apps by price
```
paid_apps=data_clean[data_clean['Type']=='Paid']
free_apps=data_clean[data_clean['Type']=='Free']
plt.figure(figsize=(8,6))
sns.histplot(data_clean['Price'], bins=20, kde=True,color='purple',label='Paid Apps')
plt.title('Distribution of Prices for Paid Apps')
plt.xlabel('Price ($)')
plt.ylabel('Frequency')
plt.legend()
plt.show()
```
![U-A](https://github.com/NECHEBLESSING/-Unveiling-the-Android-App-Market-Analyzing-Google-Play-Store-Data/blob/main/U-A%2012.PNG)

### APP UPDATED BY YEAR
The number of app updates by year was analyzed to understand the trend of app updated. The
results showed:
- Number of app updated by Year

```
plt.figure(figsize=(10,6))
data_clean['Last
Updated'].dt.year.value_counts().sort_index().plot(kind='line',
marker='o')
plt.title('Number of App Updates by Year')
plt.xlabel('Year')
plt.ylabel('Number of Updates')
plt.show()
```
![U-A](https://github.com/NECHEBLESSING/-Unveiling-the-Android-App-Market-Analyzing-Google-Play-Store-Data/blob/main/U-A%2013.PNG)

# INSIGHTS AND RECOMMENDATION
Based on the analysis, the following insights and recommendations were derived:
### Insights:
- Most apps have a rating between 4-5
- Apps that are installed 0-0.3 times are more frequent than other installed apps
- Everyone, Teen and Everyone10+ have mean rating of 4.3, while Everyone and Mature17+ have a higher quartile range than others. Adult only 18+ and unrated are very less in
number with the latter being not available
- Family apps are more in number, with the least being beauty apps
- Subway Surfer is the most downloaded app along with Google News, Galaxies of Hope,FN etc. All have a user rating of 5.0. Clash of Clans have the most number of
reviews, followed by Subway Surfers and Clash Royale
- Most apps in Playstore are free to install
- From 2017 - 2018 many apps had major updates which led to a boom in app updates between that time with more than 4000 updates.
### RECOMMENDATIONS
- Developers should focus on creating high-quality apps with good content ratings
- Developers should prioritize app updates to keep users engaged.













