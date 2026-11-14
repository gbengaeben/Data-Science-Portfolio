# Emoji Sentiment Analysis

**Are popular emojis generally associated with positive or negative sentiment?**

An end-to-end analysis of how 750 emojis are used in 1.6 million labelled tweets across 13 European languages, with a Python notebook for the analysis and an interactive web dashboard to explore the results.

### 🔗 [Live interactive dashboard](https://gbengaeben.github.io/<repo-name>/)

![Dashboard: popularity vs sentiment](figures/dashboard_popularity_vs_sentiment.png)

---

## Key findings

| Question | Answer |
|---|---|
| What share of emojis are positive? | **82.4%** of emoji types and **86.1%** of emoji uses are net-positive (threshold 0) |
| Are the most popular emojis positive? | **18 of the 20** most used emojis (90%) are net-positive |
| Which popular emojis are not? | 😭 Loudly Crying Face (-0.09) and 😩 Weary Face (-0.37) |
| Most positive emoji (more than 500 uses) | ❤ Heavy Black Heart (+0.75) |
| Most negative emoji (more than 500 uses) | 😒 Unamused Face (-0.37) |
| Where in a tweet do emojis appear? | Late: usage-weighted average position is **0.75** (0 = start, 1 = end) |
| Do positive and negative emojis sit in different places? | Yes. Negative emojis sit later (0.83) than positive ones (0.75). Difference -0.086, 95% bootstrap CI [-0.112, -0.060] |

Things worth noting:

- **Popular does not always mean positive.** 😭 is a top-5 emoji but net negative, because people use it for both joy and sadness.
- **Even the most used emoji is only mildly positive.** 😂 has a net sentiment of +0.22: 47% of the tweets it appears in are positive, 29% neutral and 25% negative.
- **The answer depends on the definition.** At a stricter threshold, fewer emojis count as clearly positive (see the sensitivity table below).

Net sentiment = share of positive tweets minus share of negative tweets among tweets containing the emoji (range -1 to +1).

## The dataset

The data (`data/emoji-sentiment.csv`) gives the sentiment associated with each emoji. Researchers had annotators label 1.6 million tweets in 13 European languages as positive (+1), neutral (0) or negative (-1), then summarised the labels per emoji. Only about 4% of those tweets contained emojis, and only emojis with at least 5 occurrences are included.

| Column | Meaning |
|---|---|
| `Char`, `Unicode name`, `Unicode codepoint` | The emoji and its identifiers |
| `Occurrences [5...max]` | Number of times the emoji appears (5 to 14,622) |
| `Position [0...1]` | Average position in a tweet (0 = start, 1 = end) |
| `Neg`, `Neut`, `Pos` | Share of tweets containing the emoji that are negative, neutral, positive |

**Source and credit:** Emoji Sentiment Ranking, Kralj Novak, Smailović, Sluban and Mozetič, "Sentiment of Emojis", *PLOS ONE*, 2015. Check the dataset's licence before reusing or redistributing it.

## Method

1. **Sanity checks.** Missing values, duplicates, value ranges, and whether the three sentiment shares sum to 1.
2. **Cleaning.** Explicit `snake_case` column names, removal of empty and redundant columns (`Image [twemoji]`, an empty confidence-interval column), and removal of one non-emoji row (the Unicode replacement character U+FFFD). This leaves 750 emojis.
3. **New variables.**
   - `sentiment` = `pos` - `neg`
   - `positive_flag` = `sentiment > 0`
   - `sentiment_class` = positive / mixed-neutral / negative, using a ±0.10 threshold
4. **Usage-weighted statistics.** Each row is one emoji type, so a rare emoji with 5 uses counts the same as 😂 with 14,622 uses in a plain average. Results are reported both per emoji type and weighted by how often each emoji is used.
5. **Threshold sensitivity.** The share of "positive" emojis is recomputed at several thresholds.
6. **Bootstrap confidence interval.** The difference in average position between positive and negative emojis is tested by resampling emoji types 5,000 times (usage-weighted), instead of relying on a single point estimate.

### Sensitivity to the positivity threshold

| Threshold | % of emoji types positive | % of emoji uses positive |
|---|---|---|
| 0.00 | 82.4 | 86.1 |
| 0.05 | 80.3 | 83.8 |
| 0.10 | 76.1 | 82.8 |
| 0.20 | 64.3 | 77.7 |
| 0.30 | 52.1 | 64.3 |

## Interactive dashboard

The dashboard (`docs/index.html`) is a single self-contained web page built with plain JavaScript and [Chart.js](https://www.chartjs.org/), with the data embedded so nothing needs to be uploaded.

**Controls:** a sentiment threshold slider, a minimum-uses filter, and a toggle between counting by usage or by emoji type. Every chart and number updates instantly.

**Tabs:**

- **Overview:** share of positive, mixed and negative emojis, by type and by usage
- **Popularity vs sentiment:** every emoji as a dot (hover for details), with the 12 most used labelled
- **Extremes:** the 7 most positive and 7 most negative emojis
- **Placement:** where positive and negative emojis sit in tweets, with the bootstrap interval
- **Explorer:** a filterable, sortable table of the data
- **About:** definitions and limitations

![Dashboard: extremes](figures/dashboard_extremes.png)

## Repository structure

```
.
├── README.md
├── requirements.txt
├── emoji_sentiment.ipynb      # analysis notebook
├── data/
│   └── emoji-sentiment.csv    # source data
├── figures/                   # charts and dashboard screenshots
└── docs/
    └── index.html             # interactive dashboard (served by GitHub Pages)
```

## How to run

**In Google Colab:** open the notebook, then run all cells. If the CSV isn't found, an upload dialog opens: choose `emoji-sentiment.csv`.

[![Open in Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/gbengaeben/<repo-name>/blob/main/emoji_sentiment.ipynb)

**Locally:**

```bash
git clone https://github.com/gbengaeben/<repo-name>.git
cd <repo-name>
pip install -r requirements.txt
jupyter notebook emoji_sentiment.ipynb
```

The notebook looks for `emoji-sentiment*.csv` in the working directory or in `data/`. The last part of the notebook contains an interactive widget dashboard (`ipywidgets` and `plotly`), which works in Colab and Jupyter. The web dashboard in `docs/` needs no setup: open `docs/index.html` in a browser (it loads Chart.js from a CDN, so it needs an internet connection).

## Limitations

- Only emojis with at least 5 occurrences are included, so rare emojis are missing.
- The tweets are in 13 European languages, so the results may not generalise to other languages or regions.
- Sentiment labels belong to the whole tweet, not to the emoji alone. "Emoji sentiment" here means the sentiment of the tweets an emoji appears in.
- Only a small fraction of the original 1.6 million tweets contained emojis.
- "Positive" depends on the chosen threshold; the sensitivity table shows how much.

## Tech stack

Python, pandas, NumPy, Matplotlib, seaborn, Plotly, ipywidgets, Jupyter / Google Colab, JavaScript, Chart.js, GitHub Pages.

## Possible next steps

- Extend the analysis to emoji skin tones and sequences
- Compare sentiment by language
- Model sentiment from emoji features, with cross-validation
- Compare these results with newer emoji sentiment datasets

## Author

**Fakiyesi Ebenezer**: [LinkedIn](https://www.linkedin.com/in/fakiyesi-ebenezer-a69879305/) · [GitHub](https://github.com/gbengaeben)
