# Social Media Sentiment & Engagement Analysis

## 1. Project Overview

This project analyzes a social-media dataset containing post text, sentiment/emotion labels, timestamps, users, platforms, hashtags, engagement metrics, and country information.

The goal is to understand how sentiment and other contextual factors relate to social-media engagement.

Visuals link: https://public.tableau.com/app/profile/joshua.ikem/viz/Book1_17508045453400/Dashboard5?publish=yes

### Main analytical question

**How does sentiment relate to social-media engagement, and how does this relationship vary across platforms, countries, and time?**

---

## 2. Dataset

The source dataset contains the following fields:

- `Unnamed: 0.1` — exported index column
- `Unnamed: 0` — exported index column
- `Text` — social-media post text
- `Sentiment` — source sentiment/emotion label
- `Timestamp` — post timestamp
- `User` — user/account identifier
- `Platform` — social-media platform
- `Hashtags` — hashtags associated with the post
- `Retweets` — retweet count
- `Likes` — like count
- `Country` — country associated with the post
- `Year`, `Month`, `Day`, `Hour` — time components

The two `Unnamed` columns are removed because they are exported index fields rather than analytical variables.

---

## 3. Analytical Approach

The project follows a professional data-analysis workflow:

1. Load & inspect
2. Remove unnecessary columns
3. Clean whitespace
4. Check data types
5. Check missing values
6. Check duplicates
7. Validate categories
8. Validate numerical values
9. Validate `Timestamp` against `Year`, `Month`, `Day`, and `Hour`
10. Investigate outliers
11. Create `Total_Engagement`
12. Clean/transform hashtags
13. Prepare text
14. Exploratory Data Analysis
15. Insights
16. Recommendations

---

## 4. Data Cleaning

### Unnecessary columns

The following columns are removed:

- `Unnamed: 0.1`
- `Unnamed: 0`

### Whitespace

Leading and trailing whitespace is removed from string columns. Empty strings are converted to missing values.

This is important because values such as `" Positive"` and `"Positive "` can otherwise be treated as different categories.

### Data types

- `Timestamp` is converted to datetime.
- Engagement and time fields are converted to numeric types.

### Missing values

Missing values are profiled by both count and percentage. Rows are not blindly dropped because the correct treatment depends on the variable and the reason for missingness.

### Duplicates

Exact duplicate rows are audited and removed after inspection.

### Numerical validation

`Likes` and `Retweets` are checked for invalid negative values. Descriptive statistics are used to understand their distributions.

---

## 5. Sentiment Handling

The source dataset contains many detailed sentiment/emotion labels, including broad labels such as:

- Positive
- Negative
- Neutral

and more specific labels such as:

- Happiness
- Sadness
- Anger
- Fear
- Hope
- Curiosity
- Love
- Admiration
- Confidence
- Creativity

The project therefore keeps the original `Sentiment` column unchanged.

A second field, `Sentiment_Group`, is created to combine related labels into broader analytical families.

Examples:

- `Positive`, `Happiness`, `Joy` → `Happiness & Joy`
- `Sadness`, `Grief`, `Sorrow` → `Sadness & Loss`
- `Anger`, `Frustration`, `Resentment` → `Anger & Resentment`
- `Hope`, `Optimism`, `Gratitude` → `Hope & Optimism`

This grouping reduces fragmentation during EDA while preserving the original source labels.

**Important:** `Sentiment_Group` is a manually defined analytical grouping. It should not be interpreted as a machine-learning sentiment prediction or as a definitive positive/negative polarity score.

---

## 6. Feature Engineering

### Total Engagement

A new metric is created:

`Total_Engagement = Likes + Retweets`

This provides a simple combined measure of visible post engagement.

### Hashtag features

The hashtag field is transformed into:

- `Hashtag_List`
- `Hashtag_Count`

Hashtags are also exploded into individual observations so frequency and engagement can be analyzed at hashtag level.

### Text features

The project creates:

- `Word_Count`
- `Character_Count`

These are used for basic text-length analysis. This stage does not attempt to build a sentiment classifier.

### Continent

Countries are mapped to broad continents to support geographic analysis.

Because some countries have very small sample sizes, continent-level comparisons should be interpreted more cautiously than large-sample groups.

---

## 7. Exploratory Data Analysis

The notebook investigates:

### Sentiment
- Distribution of sentiment groups
- Average engagement by sentiment group
- Median engagement by sentiment group
- Total engagement by sentiment group

### Platform
- Number of posts by platform
- Sentiment composition by platform
- Engagement by platform

### Geography
- Country distribution
- Continent distribution
- Sentiment composition by continent
- Engagement by continent

### Time
- Engagement by hour
- Engagement by month
- Timestamp consistency

### Engagement
- Likes vs. retweets
- Mean, median, and total engagement
- Outlier investigation

### Hashtags
- Most frequent hashtags
- Hashtag-level engagement, subject to a minimum post-count threshold

### Text
- Word count
- Character count
- Relationship between text length and engagement

---

## 8. Outlier Strategy

The notebook uses the IQR method to flag potential outliers in likes and retweets.

Outliers are **not automatically deleted**.

This is intentional. A very high engagement value may represent a legitimate viral post rather than a data error.

Any outlier decision should therefore consider:
- Whether the value is technically valid
- Whether it is plausible
- Whether it represents a real event
- Its effect on the analysis

---

## 9. Key Questions

The analysis is designed to answer:

1. What sentiment groups dominate the dataset?
2. Which sentiment groups receive the highest average engagement?
3. Which platforms have the highest engagement?
4. Does sentiment composition differ across platforms?
5. How does engagement vary across countries and continents?
6. Which posting hours are associated with higher engagement?
7. Does engagement vary across months?
8. Are likes and retweets strongly related?
9. Which hashtags are most common?
10. Which hashtags are associated with higher engagement?
11. Does post length appear to relate to engagement?

---

## 10. Insights

The notebook intentionally does not hard-code conclusions.

Insights should be written after the analysis has been executed and should include:

**Finding → Evidence → Interpretation → Caveat**

For example:

> The [group] sentiment category recorded the highest average engagement, with an average of [value] interactions per post. This suggests that posts associated with this emotional category may attract stronger engagement in this dataset. However, the result should be considered alongside sample size and outliers.

This approach prevents unsupported claims.

---

## 11. Recommendations

Recommendations should be based on the actual results. Possible recommendation areas include:

### Content strategy
Identify emotional/content categories associated with consistently strong engagement.

### Platform strategy
Adapt content strategy when engagement patterns differ materially between platforms.

### Timing strategy
Test posting during time windows associated with higher average engagement.

### Hashtag strategy
Evaluate hashtags based on both frequency and engagement rather than frequency alone.

### Data quality
Maintain validation rules for timestamps, categories, missing values, duplicates, and engagement metrics.

### Further analysis
Use statistical testing or regression models to determine whether observed relationships remain after accounting for platform, geography, and time.

---

## 12. Important Limitations

1. The dataset is observational, so associations should not be interpreted as proof of causation.
2. Engagement can be affected by factors not included in the dataset.
3. Some countries have very small numbers of observations, making country-level averages unstable.
4. High-engagement observations may be legitimate viral posts rather than errors.
5. The manually created `Sentiment_Group` is an analytical categorization, not a model-generated sentiment score.
6. The dataset's timestamp components should be treated as derived fields and validated against `Timestamp`.

---

## 13. Project Structure

Recommended repository structure:

```text
sentiment-analysis/
│
├── data/
│   ├── Sentiment dataset.csv
│   └── sentiment_cleaned.csv
│
├── notebooks/
│   └── sentiment_analysis_rebuilt.ipynb
│
├── README.md
│
└── requirements.txt
```

---

## 14. Tools

- Python
- Pandas
- NumPy
- Matplotlib
- Seaborn
- Jupyter Notebook

---

## 15. How to Run

1. Clone/download the project.
2. Place the original CSV inside the `data/` directory.
3. Open `notebooks/sentiment_analysis_rebuilt.ipynb`.
4. Update `DATA_PATH` if your file has a different location.
5. Run the notebook from top to bottom.
6. Review the validation outputs before interpreting the EDA.
7. Write the final insights and recommendations from the actual results.

---

## 16. Portfolio Value

This project demonstrates a practical Data Analyst workflow rather than only visualization.

It demonstrates:

- Data ingestion
- Data profiling
- Data cleaning
- Data validation
- Missing-value analysis
- Duplicate handling
- Categorical validation
- Numerical validation
- Datetime validation
- Outlier investigation
- Feature engineering
- Text preparation
- Exploratory analysis
- Business-question formulation
- Insight generation
- Recommendation development
- Documentation

The project is therefore suitable as a portfolio case study for junior/entry-level Data Analyst applications.
