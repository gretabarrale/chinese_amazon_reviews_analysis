# Chinese Amazon Reviews: A Customer Insight Analysis

> **Status: work in progress.** Data cleaning is almost complete; the customer insight analysis is next.

## Overview

What makes a Chinese customer satisfied, dissatisfied or indifferent?

This project analyses 126,000 Chinese-language Amazon reviews to find out what customers complain about, what they appreciate, and what leads to a neutral review. Sentiment labels support the analysis, but the goal is business insight.

Working with Chinese text also meant reading it closely: in Chinese, repeating a word or a sentence can be noise, but it can also be a deliberate way of adding emphasis.

## Dataset

The Chinese subset of the [tyqiangz/multilingual-sentiment-datasets](https://github.com/tyqiangz/multilingual-sentiment-datasets) repository on GitHub: real Amazon product reviews labelled as positive, neutral or negative. The three labels are balanced by construction, so the analysis focuses on what characterises each group rather than on how large it is.

> **Note on the data source.** tyqiangz took the reviews from the Multilingual Amazon Reviews dataset on Hugging Face, kept only those with 1, 3 or 5 stars and turned the star ratings into labels: 1 star = negative, 3 stars = neutral, 5 stars = positive. Reviews with 2 or 4 stars are not included, so positive and negative reviews represent the most extreme opinions.

## What has been done so far

- **First checks:** no missing values; exact duplicates removed.
- **Repetition analysis:** some reviews repeat the same text many times. By inspecting samples of these reviews, I separated spam (the whole review is the same sentence copied over and over) from rhetorical repetition, like 重要的事情说三遍, "important things are said three times".
- **Detection and cleaning:** two functions, `spam_detector_v2` and `spam_cleaner`, tested on hand-labelled cases, flag spam reviews and reduce each one to a single copy, keeping any extra content the customer added. The reviews are cleaned, not removed, so no customer opinion is lost.
- **First finding:** customers who repeat their whole review are much less likely to be neutral (23.5% against 33.7% in the full dataset). Repetition goes with strong opinions, in either direction.

The full reasoning, examples and known limitations are documented in [`eda_cleaning.ipynb`](eda_cleaning.ipynb).

## Next steps

- Analyse the repeated reviews further. The first finding suggests that repetition is more than spam, so the terminology and the function names will be revised, and the cleaned text will be saved in a new column next to the original.
- Explore the reviews by label: length, recurring words, reviews without a real opinion.
- Group the most characteristic words of each label into themes, such as quality, delivery, price and customer service.
- Train a TF-IDF and logistic regression classifier, to confirm the themes and find reviews where the text and the rating disagree.
- Turn the results into insights: what customers complain about, what they value, and what makes a review neutral.

## How to run

1. Download `train.csv`, `valid.csv` and `test.csv` from the [`data/chinese`](https://github.com/tyqiangz/multilingual-sentiment-datasets/tree/main/data/chinese) folder of the dataset's GitHub repository and save them in `data/raw/` (create it if it does not exist).
2. Install the libraries: `pip install pandas numpy matplotlib seaborn`
3. Open `eda_cleaning.ipynb` and run all cells.

## Tools

Python, pandas, NumPy, Matplotlib, Seaborn, regular expressions (`re`).