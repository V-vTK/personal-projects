# TowardsDataMining

Coursework for the **Towards Data Mining** course at the University of Oulu.

The course covered the data mining pipeline from data acquisition and preprocessing to analysis, modeling, and interpretation — using multiple tools and programming languages depending on the task.

## Background & Motivation

This repository contains my solutions for the TDM course, where we explored the data mining process hands-on across five weekly exercises. Each week introduced a different aspect of data mining — from basic data exploration in R and MATLAB, to relational databases, sampling techniques, sensor data analysis, and missing data imputation. The course emphasized working with real-world data formats and understanding the practical challenges of preparing and analyzing data.

## Exercises

### Week 1 — Data Exploration (R & MATLAB)

- **R**: Loaded and explored a concrete compressive strength dataset. Practiced basic data operations: `head`, `tail`, `str`, `summary`, subsetting, correlation calculations, and visualization with `plot`, `hist`, and `boxplot`.
- **MATLAB**: Same analysis workflow replicated in MATLAB using `readtable`, `corr`, `summary`, and plotting functions. Built a helper function `comma2point_overwrite` to handle European decimal comma format.
- **Key skills**: Data frame manipulation, descriptive statistics, correlation analysis, understanding locale-dependent data formats.

### Week 2 — Databases & Synthetic Data

- **SQL & MySQL**: Set up a MySQL database using Docker Compose. Created relational tables for an activity monitoring system linking persons to their activity measurements with foreign keys.
- **Python (SQLite)**: Wrote a Python script to create and query a local SQLite database using the same schema.
- **Synthetic data generation**: Used NumPy to generate synthetic 2D classification data with multivariate normal distributions — two classes with different means and a shared covariance matrix. Visualized the generated data with matplotlib scatter plots.
- **Key skills**: Relational database design, SQL queries, Docker for reproducible environments, synthetic dataset creation.

### Week 3 — Sensor Data Visualization

- **MATLAB**: Loaded and plotted gyroscope sensor data from a text file. Extracted individual axes (X, Y, Z) and plotted observations against their index to visualize sensor readings over time.
- **Key skills**: Time-series data handling, sensor data parsing, MATLAB plotting.

### Week 4 — Weather Data & Sampling Techniques

- **Task 1 — Weather Data Loading**: Worked with three weather station datasets in different formats — Excel, plain text, and CSV. Each contained timestamp-temperature pairs with varying separators and structures, practicing heterogeneous data import.
- **Task 2 — Sampling Methods (MATLAB)**: Implemented and compared sampling techniques for classification data:
  - **Sampling without replacement** using `randperm` to pick a subset of indices
  - **Sampling with replacement** allowing the same point to be selected multiple times
  - **Balanced sampling with replacement** ensuring class balance in the sample
  - **Balanced sampling with replacement + noise** adding Gaussian noise to sampled points
  - Helper function `cirrdnPJ` for generating random points within a circle (used for synthetic classification data)
- **Key skills**: Handling diverse file formats, understanding sampling bias, class balance, noise injection.

### Week 5 — Missing Data & Imputation

- **R**: Worked with a modified Iris dataset containing missing values:
  - **Complete case analysis** (`complete.cases`) to identify rows without missing data
  - **Mean imputation**: Replaced missing values with column means
  - **Stratified mean imputation**: Replaced missing values with means calculated per species (setosa, versicolor, virginica) — preserving class-specific distributions
  - **Linear regression**: Modeled `Petal.Width ~ Petal.Length` and analyzed fit with `lm` and `summary`
  - **Covariance analysis**: Compared covariance estimates using `complete.obs` vs `pairwise.complete.obs`
- **Key skills**: Missing data mechanisms (MCAR/MAR), imputation strategies, understanding bias-variance tradeoffs in imputation.

## Technologies Used

- **Languages:** R, MATLAB, Python, SQL
- **R:** Base R (data frames, plotting, statistics, `lm`)
- **MATLAB:** Data import, plotting, custom functions, sampling
- **Python:** NumPy, matplotlib, sqlite3
- **Databases:** MySQL (Docker), SQLite
- **Infrastructure:** Docker Compose
- **Data Formats:** CSV (comma & semicolon delimited), Excel (.xlsx), TXT, .mat files
- **Tools:** RStudio, MATLAB, VS Code

## Key Takeaways

- Gained practical experience across **four different tools** (R, MATLAB, Python, SQL) — choosing the right tool for each data mining task.
- Learned to handle **real-world data quirks**: European decimal commas, missing values, heterogeneous file formats, inconsistent delimiters.
- Understood **sampling strategies** and their impact on model training: with/without replacement, balanced sampling, noise injection.
- Practiced **missing data imputation** strategies — from simple mean imputation to stratified (class-aware) approaches.
- Built a **MySQL database in Docker** for reproducible development environments.
- Generated **synthetic classification data** with controlled statistical properties.
- Learned to **visualize sensor time-series data** and identify patterns in raw measurements.
- Worked with **relational database schemas** linking persons, activity monitors, and measurements.

## Status

- **Completed:** Yes (course exercises finished)
- **Maintained:** No (archive — coursework reference)
- **Notes:** This repository serves as a reference for data mining fundamentals. Each week's exercise is self-contained with its own data files and scripts.

## My Contributions

> *Individual coursework*

## Links

- Course: Towards Data Mining, University of Oulu

## Further Notes

This document was created by giving an AI access to the source code and course materials from each week's exercises. The report was then edited and verified.