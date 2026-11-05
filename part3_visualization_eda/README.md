# Part 3 - Visualization, EDA and Business Thinking

## Data source
Trending YouTube Video Statistics (Kaggle: datasnaek/youtube-new). This dataset is not my own work.
Countries: US, GB, IN, plus categories.csv.

## Data preparation
The original CSVs were trimmed by dropping the description, tags and thumbnail_link columns to stay under GitHub's size limit. categories.csv was built from US_category_id.json.

## How to run
1. Open part3_visualization_eda.ipynb in Google Colab.
2. Runtime -> Run all.
3. The first cell clones this repository (branch part3-eda) so the data/ folder is available.
Requirements: Python 3, numpy, pandas, matplotlib, seaborn, plotly (all pre-installed in Colab).

## Data profile and cleaning
- Combined shape: 117,217 rows, 15 columns. No column has missing values (0%).
- Removed 5,118 duplicates (same video_id, trending_date, country), 94 records of removed videos and corrupted video_ids starting with #.
- Final data: 111,812 records.
- days_to_trend = days from publish_time to trending_date (median 4 days; negative values set to 0).

## Charts and interpretations
Charts 1-5 use Matplotlib/Seaborn, charts 6-7 use Plotly. Each interpretation is written in the notebook Markdown cells under the chart:
1. Bar: records per category (Entertainment and Music are the top two).
2. Histogram: log10 views (one peak near 5.7, thin right tail).
3. Scatter: views vs likes (log-log, strong upward relationship).
4. Line: trending records per day (about 550-600 per day).
5. Box plot: log views by category.
6. Plotly scatter: views vs likes by category, title on hover.
7. Plotly grouped bar: average views per category by country.

## Distributions
- Views: mean 3,180,743 > median 556,901 > mode 191,321, so right-skewed.
- Likes: mean 80,582 > median 12,430 > mode 0, so right-skewed. The mode of 0 comes from videos with no likes or disabled ratings.

## Correlation
The two strongest correlations are views and likes (r = 0.79) and likes and comment_count (r = 0.77). Correlation does not prove that likes cause views: a popular video gets both, and a third factor such as promotion or audience size can drive both.

## Retention
Day 2, 3, 5 and 7 percentages of videos still on the trending list are shown in the notebook retention table, with curves comparing US, GB and IN.

## Cohort view
Videos were grouped by the week they first trended. The first
