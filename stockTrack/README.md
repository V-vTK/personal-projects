# stockTrack

A full-stack personal portfolio tracking application for managing investments across multiple brokers, with tax reporting and AI-assisted features. Built as a solo personal project to handle my book-keeping, replacing spreadsheets and fragmented broker dashboards.

## Background & Motivation

I manage investments across multiple brokers — some Finnish, some large global brokers. I found it tedious to track overall performance, calculate taxes, and get a clear picture of my portfolio. Each broker had its own UI, reports, and data formats, and Finnish tax reporting required manual calculations across all of them. This project consolidates everything into one place: book-keeping, taxes, and withholding tax reclaims (Denmark, Norway, Canada).

## Features

- **Multi-broker support** — Import transactions from various brokers via CSV ledger uploads
- **Transaction management** — Record buys, sells, dividends, deposits, withdrawals, interest, cashback, staking, and more
- **FIFO-based tax calculation** — Automated cost basis and gain/loss calculation using First-In-First-Out matching
- **Tax PDF export** — Generate pre-filled Finnish tax forms (Verottaja-compatible PDF) with dividend withholding tax reclaims across multiple countries (Denmark, Norway, etc.)
- **Portfolio dashboard** — Charts and visualizations using Recharts: account pie charts, portfolio performance over time, open positions, and asset allocation
- **Dividend & split handling** — Track dividends, handle stock splits, and calculate withholding tax reclaims
- **Multi-currency support** — Automatic daily FX rate handling for transactions in different currencies
- **Crypto staking imports** — Support for crypto staking income entries
- **AI assistant integration** — Ollama-powered AI features accessible from the UI for data analysis and assistance
- **CI/CD pipeline** — Automated Docker-based deployment via Jenkins
- **Test automation** — Selenium-based end-to-end tests running in Docker

## Screenshots

![Portfolio Dashboard](./.vscode/stockTrack/image.png)

> *Additional screenshots may be added later.*

## Technologies Used

- **Language:** TypeScript, Python
- **Frontend:** Next.js 15 (App Router, Turbopack), React 19, Tailwind CSS 4, ShadCN UI (Radix primitives)
- **Backend:** PocketBase (Go-based lightweight backend with SQLite, real-time subscriptions, and file storage)
- **Charts:** Recharts
- **State Management:** Zustand
- **Forms & UI:** react-day-picker, react-dropzone, react-to-print, Sonner (toasts), cmdk (command palette)
- **PDF generation:** @react-pdf/renderer
- **CSS:** Tailwind CSS, class-variance-authority, clsx, tailwind-merge, lucide-react icons
- **AI:** Ollama integration for AI-assisted features
- **Testing:** Python, pytest, Selenium (undetected-chromedriver)
- **CI/CD:** Jenkins, Docker, Docker Compose
- **Other:** PocketBase hooks (JavaScript), Yahoo Finance API for price data

## Key Takeaways

- Gained deep experience with **PocketBase** as a backend-as-a-service — schema design, hooks, auth, and real-time features. The flexibility of having the backend embedded in Docker was a huge productivity boost.
- Implemented a **FIFO ledger engine** (`TransactionLedger.ts`, `fifo.ts`) from scratch to track cost basis and match buys with sells across multiple accounts and currencies — one of the more algorithmically interesting parts of the project. Ledger transactions are tricky because all history matters for correct cost basis and FIFO grouping.
- Learned to handle **multi-currency tax reporting** in the Finnish system, including withholding tax reclaims for foreign dividends (Denmark, Norway, US). The tax PDF export logic encodes real tax rules and reclaim procedures.
- Set up a **full CI/CD pipeline** with Jenkins and Docker, including environment injection, health checks, and automated test execution.
- Built a **CSV import pipeline** for multiple broker formats, each with different column layouts and data conventions.
- The project reinforced the value of **good type definitions** — the TypeScript interfaces (`Transaction.ts`, `Account.ts`, `Assets.ts`) were designed to match the PocketBase schema closely, keeping the frontend and backend in sync.
- Used **OOP** to organize ledger and transaction logic into clean, testable classes rather than scattered utility functions.
- Working with **Next.js 15 App Router** with server components, middleware for auth, and Turbopack for fast dev iterations.

## Status

- **Completed:** In progress (actively developed, quite well featured and missing only minor things)
- **Maintained:** Yes (personal use)
- **Notes:** The project is a living tool that evolves with my portfolio. Some features (like AI-powered transaction photo recognition) are experimental. The tax reporting logic is specifically tailored to Finnish tax rules and may need updates as regulations change.

## My Contributions

> *This was a solo project.*

## Links

- Inspired by / uses [PocketBase](https://pocketbase.io/)
- Uses [ShadCN UI](https://ui.shadcn.com/) component library
- Uses [Next.js](https://nextjs.org/)

## Further Notes

This document was created by giving an AI access to the source code. The report was then edited and verified.