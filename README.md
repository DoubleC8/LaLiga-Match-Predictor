# La Liga Match Predictor: End-to-End Scraping & Machine Learning

An end-to-end data science pipeline that scrapes historical La Liga football data from FBref, engineers custom features, and predicts match outcomes (Wins/Losses) using a Random Forest Classifier.

## Project Overview

This project was built to test the viability of predicting football match outcomes using historical stats. It is divided into two distinct phases:

1. **The Web Scraper:** Navigates FBref's Cloudflare security to systematically scrape match and shooting statistics for La Liga teams across multiple seasons (2024–2026).
2. **The ML Model:** Cleans the scraped data, calculates team form (rolling averages), and uses `scikit-learn` to identify high-confidence win predictions.

## Key Features

- **Advanced Web Scraping:** Uses `undetected_chromedriver` and `BeautifulSoup` to bypass aggressive Cloudflare anti-bot protection and parse complex HTML tables.
- **Feature Engineering:** Calculates 3-game rolling averages for key indicators of form (Goals For, Goals Against, Shots on Target).
- **Dual-Sided Predictions:** Instead of evaluating teams in a vacuum, the model merges predictions for both the Home and Away teams. It isolates "High-Confidence" matches where the model simultaneously predicts Team A to win and Team B to lose.
- **Custom Data Mapping:** Programmatically handles discrepancies between URL-formatted team names and UTF-8 Spanish accents (e.g., _Atletico Madrid_ vs. _Atlético Madrid_).

## Tech Stack

- **Python 3**
- **Pandas** (Data manipulation and cleaning)
- **Scikit-Learn** (Random Forest Classifier, Precision Metrics)
- **Undetected-Chromedriver / Selenium** (Headless browser automation)
- **BeautifulSoup 4** (HTML parsing)

## Setup & Installation

1. Clone this repository.
2. Install the required Python packages:
   ```bash
   pip install pandas scikit-learn undetected-chromedriver beautifulsoup4 lxml html5lib
   ```

## Results & Evaluation

- Baseline Precision: ~50.5%
- Final High-Confidence Precision: 52.3%
  While predicting a 3-way outcome sport (Win/Loss/Draw) is difficult; especially in a heavily tactical league like La Liga that features a high volume of draws—breaking the 52% precision mark proves the model successfully learned underlying patterns.

Lessons Learned: During development, we discovered that feeding the model rare, "noisy" stats (like penalty kicks) caused overfitting and decreased accuracy. Stripping the features down to core form metrics (Goals and Shots on Target) stabilized the model.

## Future Improvements

To push the precision score closer to 60%, future iterations of this project could include:

1. Expanding the Dataset: Scraping data going back to 2018 to give the Random Forest more historical patterns to learn from.
2. Integrating xG (Expected Goals): Replacing raw goals with rolling xG and xGA to better measure the true quality of a team's recent performances.
3. Hyperparameter Tuning: Experimenting with GridSearch to optimize the Random Forest's n_estimators and min_samples_split.
4. Including Possession Metrics: Because La Liga is a highly possession-oriented league, adding rolling possession percentages could be a strong predictor of match control.
