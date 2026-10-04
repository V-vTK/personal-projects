# ValueInvestingTemplate

A collection of automated tools for equity research and commodity market analysis. Scrapes fundamental financials into structured Excel reports, augments them with AI-generated investment analysis (Gemini), and tracks CFTC Commitment of Traders data against commodity prices with Discord alerts. Deployed on a home server via Docker Compose and Jenkins CI/CD.

## Background & Motivation

I wanted a systematic way to analyze stocks without clicking through various data sources manually. Instead of scattered spreadsheets, this pulls financial data into structured Excel reports with optional AI commentary. The commodities side came from wanting to visualize what commercial traders (smart money) are doing in futures markets.

## What It Does

1. **Morningstar scraper** — Uses Playwright (Firefox) to authenticate and call an API for key metrics, dividends, ownership, valuation ratios, historical prices, earnings transcripts, and sustainability scores. Data is cached locally.
2. **Excel report generator** — Populates pre-formatted `.xlsx` templates with valuation tables, ratio comparisons, and price charts.
3. **AI research analyst** — Sends scraped data to Google Gemini with a structured prompt to generate a professional investment thesis (business overview, financial health, valuation, risk assessment).
4. **Commodities COT dashboard** — Streamlit dashboard plotting CFTC commercial trader net positioning against prices for gold, silver, copper, crude oil, palladium, and platinum.
5. **Discord COT alerts** — APScheduler cron that checks CFTC data weekly (Fridays at 4 PM) and posts notable positioning signals to a Discord channel.
6. **Docker Compose + Jenkins** — All services (Morningstar UI, commodities dashboard, Discord cron) run in containers.

## Key Takeaways

- COT data comes from CFTC's legacy HTML tables; parsing disaggregated producer/merchant/swap dealer positions across asset classes was the trickiest part.
- A well-structured Gemini prompt produces surprisingly solid equity research when given enough context and a strict output template.
- The Excel template pattern (pre-formatted `.xlsx` with named cells) is pragmatic: the data layer fills in cells rather than generating sheets from scratch.