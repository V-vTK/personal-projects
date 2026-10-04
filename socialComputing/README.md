# SocialComputing

A collection of programming assignments for the **Social Computing** course at the **University of Oulu**. Completed as part of the course curriculum, covering SQL, social media platform data analysis, content moderation, topic modelling, sentiment analysis, and platform feature design.

## Background & Motivation

This repository contains my coursework for the Social Computing course, where we explored social media platforms from a computational perspective. The course involved working with an SQLite dataset from a mock social media platform called **Mini Social** — a Flask-based web app with users, posts, comments, reactions, and follows. The weekly assignments progressed from basic data exploration to advanced NLP techniques and culminated in designing and implementing a new social feature.

The dataset contained tables like `users`, `posts`, `comments`, `reactions`, and `follows`, which were used throughout the exercises.

## Assignments

### Week 1 — Data Exploration & SQL Analysis

- **Database Inspection:** Loaded the SQLite database using `sqlite3` and `pandas`, inspected all tables with `PRAGMA table_info`, and described column names, types, and sample data.
- **Lurkers:** Used `LEFT JOIN` queries to identify users who have never posted, commented, or reacted (but may follow others). Found users with zero content engagement.
- **Influencers:** Identified the top 5 most engaging users by summing reactions + comments on their posts using nested subqueries.

### Exercise 1.4 — Spammers
- Detected users who posted or commented the same exact content at least 3 times using `GROUP BY` + `HAVING COUNT(*) >= 3` across both `posts` and `comments` tables.

### Exercise 1.5 — Database Events
- Found the most recent timestamp across `users`, `posts`, and `comments` tables, then calculated the time delta to "now" to synchronise all timestamps forward.

### Week 2 — Growth, Virality & Engagement

- **Platform Growth:** Plotted cumulative user growth and transactions per year using Matplotlib. Estimated future server capacity needs based on average yearly growth and top user locations (Boston, Germany, Melbourne, France, Tokyo).
- **Virality:** Identified the top 3 most viral posts by counting unique users who interacted (comment or reaction) on each post.
- **Content Lifecycle:** Calculated the average time between a post's creation and its first/last engagement (comments only, as reactions lacked timestamps).
- **Connections:** Found the top 3 user pairs who engage with each other's content the most, combining bidirectional interactions (A→B + B→A) using `JOIN` on mirrored pairs.

### Week 3 — Content Moderation & User Risk Analysis

- **Censorship Engine:** Implemented `moderate_content()` — a multi-tier content moderation system:
  - **Tier 1 (Severe):** Exact keyword match → immediate full content removal with `[content removed due to severe violation]` message and risk score 5.0.
  - **Tier 2 (Spam/Scam):** Phrase detection → removal with `[content removed due to spam/scam policy]` and risk score 5.0.
  - **Tier 3 (Mild Profanity):** Keyword detection → character-by-character masking with `*` and score increment of 2.0 per match.
  - **External Links:** URL detection using `urlparse` → replacement with `[link removed]` and +2.0 score per link.
  - **Excessive Capitalization:** If >70% of characters are uppercase in content longer than 15 characters → +0.5 score.
  - **Custom Rule — Excessive Punctuation:** If >10% of characters are `!` or `?` in content longer than 15 characters → +0.5 score (my own addition).
- **Risk Classification:** Scores mapped to levels: NONE (<1.0), LOW (1.0–2.99), MEDIUM (3.0–4.99), HIGH (≥5.0).
- **User Risk Analysis:** Scored all users based on their profile descriptions and content history. Identified the top 5 highest-risk users using aggregated scoring across posts, comments, and profiles.
- **Mini Social Integration:** The `moderate_content()` function was also called in the Flask app (`app.py`) to filter all visible post and comment content in the feed.

### Week 4 — Topic Modelling, Sentiment & Platform Design

- **Topic Discovery (LDA):** Used `gensim`'s Latent Dirichlet Allocation to identify the 10 most discussed topics on the platform. Pipeline: regex preprocessing → tokenisation → stop word removal → lemmatisation (spaCy) → dictionary creation → LDA model training.
- **Sentiment Analysis (VADER):** Applied NLTK's `SentimentIntensityAnalyzer` (VADER) to all posts and comments to measure the overall tone of the platform.
- **Learning from Others' Mistakes:** Researched real-world social platform failures:
  - **Discord's database evolution** (MongoDB → Cassandra → ScyllaDB) and the tombstone issue that caused JVM "stop-the-world" errors.
  - **Slack's 2025 outage** caused by database shard issues leading to API breakdowns.
  - Drafted recommendations for Mini Social: monitor API latency/errors, plan database migration ahead of time, use blackbox testing alongside old systems.
- **New Social Feature (Exercise 4.4):** Designed and implemented a new social feature for Mini Social with a UI addition (functionality demonstrated in a recorded video submission).

## Technologies Used

- **Language:** Python 3.11
- **Data Processing:** pandas, pandasql, numpy, sqlite3
- **NLP & ML:** gensim (LDA topic modelling), NLTK (VADER sentiment analysis), spaCy (lemmatisation)
- **Visualisation:** Matplotlib
- **Web Framework:** Flask (Mini Social platform)
- **Security:** cryptography (Fernet), werkzeug.security, hashlib
- **Environment:** Jupyter Notebooks / Python scripts

## Key Takeaways

- Gained hands-on experience with **SQL-based social media data analysis** using complex joins, subqueries, and aggregations to find lurkers, influencers, spammers, and viral content.
- Implemented a **multi-tier content moderation system** with automated risk scoring and censorship rules, then integrated it into a live Flask application.
- Built an **LDA topic model** from scratch using gensim, with a full NLP preprocessing pipeline (tokenisation, stop word removal, lemmatisation).
- Performed **sentiment analysis** across thousands of social media posts and comments using VADER.
- Researched **real-world platform failures** (Discord, Slack) to derive actionable engineering recommendations.
- Worked extensively with **pandasql** to run SQL queries directly on pandas DataFrames, bridging the gap between SQL and Python-based analysis.
- Improved understanding of **social media platform dynamics** — growth modelling, virality metrics, engagement lifecycle, and user risk profiling.

## Status

- **Completed:** Yes (course assignments finished)
- **Maintained:** No (archive — coursework reference)
- **Notes:** This repository serves as a reference for social computing concepts — from data exploration and NLP to content moderation and platform design. The exercises were graded as part of the Social Computing course at the University of Oulu.

## My Contributions

> *Coursework completed individually*

## Links

- Original exercise platform: [Mini Social](https://github.com/Crowd-Computing-Oulu/mini_social_exercise) (University of Oulu)
- Uses [gensim](https://radimrehurek.com/gensim/)
- Uses [NLTK](https://www.nltk.org/) (VADER)
- Uses [spaCy](https://spacy.io/)
- Uses [pandas](https://pandas.pydata.org/)
- Uses [Flask](https://flask.palletsprojects.com/)
- Course: Social Computing, University of Oulu

## Further Notes

This document was created by giving an AI access to the source code. The report was then edited and verified.