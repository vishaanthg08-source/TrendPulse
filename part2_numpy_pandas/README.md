# Part 2 - NumPy and Pandas Data Wrangling

## Data source
Trending YouTube Video Statistics (Kaggle: datasnaek/youtube-new). This dataset is not my own work.
Countries used: US, GB, IN (117,217 records before cleaning), plus categories.csv.

## Data preparation
The original CSVs were trimmed by dropping the description, tags and thumbnail_link columns to stay under GitHub's file size limit. categories.csv was built from US_category_id.json. The trimmed files are in the data/ folder of this Part.

## How to install and run
1. Open part2_numpy_pandas.ipynb in Google Colab.
2. Runtime -> Run all.
3. The first cell clones this repository (branch part2-pandas), so the data/ folder is available. No other files or network calls are needed.
Requirements: Python 3, numpy, pandas (both pre-installed in Colab).

## NumPy work
- views, likes, dislikes and comment_count were loaded into NumPy arrays (117,217 values each, dtype int64).
- Indexing and slicing: top 10 views, every 100th record, and a US sub-array.
- Like ratio and comment rate use zero-safe division (np.divide with where=).
- Views, likes and comments were min-max normalized to 0-1.
- log10(views + 1) was used because raw views are very skewed (a few videos have hundreds of millions of views), which squeezes a plot against one side.
- Top 10% of records by engagement were flagged with a boolean mask (90th percentile cutoff = 0.0182, 11,722 records).
- A days x countries array (205 x 3) of daily trending counts was reshaped, flattened and used for broadcasting (country share of each day's total, and each day's value relative to the country's average).

## Engagement score weights
engagement = 0.2 * views_norm + 0.3 * likes_norm + 0.5 * comments_norm
- Views (0.2): lowest weight, because watching is passive.
- Likes (0.3): medium weight, because a like is a deliberate action.
- Comments (0.5): highest weight, because writing a comment takes the most effort and shows real interest.
- Each column is normalized first because views, likes and comments are on very different scales.
- The score is right-skewed: median 0.00145, mean 0.00821.

## Data-quality issues and how they were handled
- Duplicates: a duplicate means the same video_id, trending_date and country. 5,118 rows were removed (kept first), giving 112,099 records.
- Removed or errored videos: 103 rows flagged video_error_or_removed. Kept.
- Comments disabled (2,520) and ratings disabled (1,222): kept. Zero likes and comments are expected there, and ratios use zero-safe division.
- Zero likes: 1,225 rows, mostly the ratings-disabled ones. Kept.
- Corrupted video_ids (for example #NAME? and #VALUE!, caused by spreadsheet errors) were excluded from the lifespan and channel summaries, because many different videos share them.
- days_to_trend: 600 values of -1 (trending_date has no time part) were clipped to 0.
- Dates: trending_date (yy.dd.mm, for example 17.14.11 = 14 Nov 2017) and publish_time (ISO text, UTC) were converted to datetime.
- Category IDs 23 and 34 are both named "Comedy" in categories.csv.

## Missing values and duplicates
- Missing values: checked every column with isna(). No column has missing values (0.0%), so no fill or drop was needed. fillna(0) and dropna() are demonstrated on a 5-row copy.
- Duplicates: removed with drop_duplicates(subset=[video_id, trending_date, country]). 5,118 rows removed.

## Category merge
categories.csv was merged on category_id with a left merge, so every trending record is kept. Rows before and after were both 112,099 and no category names were missing, so no records were lost.

## Main findings
- 27,281 records have 1M+ views and more than 50K likes.
- The top 20 records by engagement are all the BTS "FAKE LOVE" video on different days in US and GB.
- 18,637 records are in Music or Gaming with views above the overall median (557,293).
- Entertainment (29.9%) and Music (20.8%) make up over half of all records. Music has the highest average views among large categories (9.35M).
- GB has the highest average views (5.9M) and India the lowest (0.96M).
- Friday has the highest average views (4.89M), but by median Thursday (650K) and Friday (647K) are almost equal. Hours are in UTC.
- Median days from publish to trending is 4. Median trending lifespan is 2 days, and the longest videos stayed 37-38 days (GB).
- Gaming (8.97 days), Music (8.05) and Pets & Animals (8.00) stay longest. News & Politics (2.65) and Shows (1.68) disappear fastest.
- Study IQ education has the most trending videos (169). Stephen Colbert's channel has the highest total views among the top 10 channels (159M).
