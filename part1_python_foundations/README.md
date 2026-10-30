# Part 1 - Python Foundations

## Data source
Trending YouTube Video Statistics (Kaggle: datasnaek/youtube-new). This dataset is not my own work.

## Files and countries
- Countries: US, GB, IN
- data/USvideos.csv, data/GBvideos.csv, data/INvideos.csv, data/categories.csv
- Total records loaded: 117,217 (US 40,949, GB 38,916, IN 37,352)

## Data preparation
The original CSVs were trimmed by dropping the description, tags and thumbnail_link columns to stay under GitHub's size limit. categories.csv was built from US_category_id.json. This was done in a separate scratch notebook (data_prep).

## How to run
1. Open part1_python_foundations.ipynb in Google Colab.
2. Runtime -> Run all.
3. The first code cell clones this repository so the data/ folder is available.
Requirements: Python 3, pandas (pre-installed in Colab). Times in the run header are UTC.

## Cleaning rules
- views, likes, dislikes, comment_count converted to int. A bad value drops the record.
- True/False flags converted to bool. A bad value becomes False.
- trending_date (yy.dd.mm) and publish_time (ISO) parsed with datetime.strptime.
- Result: 117,217 records kept, 0 dropped, 0 parse failures.
- Category IDs 23 and 34 are both named "Comedy".

## Classification thresholds
- Unrated: ratings disabled
- Low Engagement: zero views, zero likes and dislikes, or fewer than 100,000 views
- Divisive: like ratio below 0.60
- Viral Hit: 1,000,000+ views and like ratio 0.90 or more
- Steady Performer: 100,000+ views otherwise
