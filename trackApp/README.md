# trackApp

A comprehensive personal life management application that integrates note-taking, calendar, expense tracking, asset tracking, life scoring, and more — all in one unified platform. Built as a solo personal project to consolidate the digital tools I use daily.

## Background & Motivation

I was juggling multiple apps and tools for different parts of my life — notes, calendar, budget, habit tracking, goal setting, receipts — and none of them talked to each other. I wanted a single integrated system where my calendar could show my expenses, my notes could link to my goals, and my daily life tracking could sit next to my financial data. This project is my attempt at building that unified dashboard. This was my fullstack project 10 credits graded on pass / fail system (pass).

## Features

- **Note Tracking** — Rich-text notes with Jodit editor, color coding, grouping, and execution tracking
- **Calendar** — Unified calendar view aggregating data from all modules (notes, expenses, life scores, assets, recurring events). Month, week, and day views. Google Calendar integration. Drag-to-create events and recurring event support.
- **Expense Tracking** — Log and categorize expenses with receipt OCR (PaddleOCR + Ollama). Spreadsheet-style data grid for editing. Receipt image upload with automatic text extraction and AI-powered categorization.
- **Asset Tracking** — Track assets and balances over time with per-date snapshots and growth visualizations
- **Income Tracking** — Log income entries with year integration
- **Life Tracking** — Wheel of Life dashboard with monthly scoring (1–5) across life categories (family, fitness, work, etc.). Yearly goals, weekly todos, improvement tracking, and "Future Me" profile vision.
- **Year Tracking** — Year-level summaries and notes for each year
- **Month Tracking** — Monthly summaries and reviews
- **Sport Tracking** — Track sports activities and progress
- **Link Tracking** — Save and organize links with metadata
- **In Tracking** — General inbox-style tracking
- **Fin Tracking** — Financial tracking module
- **Dashboard** — Central landing page with summary cards per module showing key metrics (monthly expenses, asset growth YTD, etc.)
- **OCR Receipt Scanning** — Docker container running Flask + PaddleOCR + Ollama for automatic receipt parsing and categorization
- **Receipt PDF Parsing** — Extract data from PDF receipts using pdf-parse
- **Image Cropping** — Built-in image cropper for receipt photos
- **Multi-user support** — User registration, authentication, and sharing
- **CI/CD pipeline** — Automated Docker-based deployment via Jenkins
- **Test automation** — Selenium-based end-to-end tests running in Docker
- **AI assistant integration** — Ollama-powered AI features for data analysis and receipt understanding

## Screenshots

<!-- ![Dashboard](./.vscode/TrackApp/example/image.png) -->

> *Screenshots may be added later.*

## Technologies Used

- **Language:** TypeScript, Python
- **Frontend:** Next.js 14 (App Router), React 18, Tailwind CSS 3, ShadCN UI (Radix primitives), Flowbite, DaisyUI
- **Backend:** PocketBase (Go-based lightweight backend with SQLite, real-time subscriptions, and file storage)
- **Charts:** Recharts
- **State Management:** Zustand
- **Editor:** Jodit (rich-text), Jodit React
- **Calendar:** Custom-built calendar components (MonthView, WeekView, DayView) with drag support
- **Spreadsheet:** react-data-grid, @silevis/reactgrid, react-table-library
- **OCR:** PaddleOCR (Python), Tesseract.js, Flask
- **AI:** Ollama integration for receipt analysis and data insights
- **Image processing:** react-image-crop, Pillow (Python), OpenCV (PaddleOCR)
- **PDF parsing:** pdf-parse (Node.js)
- **Styling:** Tailwind CSS, class-variance-authority, clsx, tailwind-merge, tailwindcss-animate, Sass
- **Testing:** Python, pytest, Selenium (undetected-chromedriver)
- **CI/CD:** Jenkins, Docker, Docker Compose
- **Other:** PocketBase hooks (JavaScript), bcrypt, ngrok, cookies-next, date-fns, dompurify

## Key Takeaways

- Built a **modular multi-app architecture** inside a single Next.js project — each "Track" (noteTrack, expenseTrack, lifeTrack, etc.) is a self-contained module with its own routes, API endpoints, types, and components. This pattern made the codebase surprisingly manageable as it grew.
- Designed a **unified calendar system** that aggregates data from 8+ different sources (notes, expenses, life scores, assets, recurring events, etc.) into a single view with color-coded event chips and source-specific filtering.
- Implemented an **OCR receipt processing pipeline** from scratch — a Flask API running PaddleOCR that sends extracted text to Ollama for structured data extraction. This involved solving real-world problems like handling Finnish receipt formats and noisy OCR output.
- Gained deep experience with **PocketBase** across multiple versions (0.27 → 0.39), including schema migrations, hooks, auth with cookies, and real-time subscriptions. The project uses PocketBase as more than just a backend — it's the integration hub connecting all modules.
- The **LifeTrack Wheel of Life** was one of the more personally meaningful features — translating a self-improvement framework into code with monthly scoring, trend charts, goal tracking, and a "Future Me" vision board.
- Learned the value of **strong typing across the stack** — the TypeScript types in `src/types/` mirror the PocketBase schema closely, and the `src/pocketbase/` module provides a typed client layer that keeps API interactions consistent.
- Set up a **full CI/CD pipeline** with Jenkins and Docker, including health checks, environment injection, and automated E2E testing.
- The project taught me how to handle **Finnish-language UI** throughout — all UI text, date formatting, and receipt parsing are in Finnish.

## Status

- **Completed:** In progress (actively developed with many modules in daily use)
- **Maintained:** Yes (personal use)
- **Notes:** This is a living tool that evolves with my needs. Some modules (like sportTrack, inTrack, finTrack) are newer and less feature-complete. The calendar module in particular has been through several iterations and is the most complex component as it connects them all. The OCR pipeline works well for Finnish receipts.

## My Contributions

> *This was a solo project.*

## Links

- Inspired by / uses [PocketBase](https://pocketbase.io/)
- Uses [ShadCN UI](https://ui.shadcn.com/) component library
- Uses [Next.js](https://nextjs.org/)
- Uses [PaddleOCR](https://github.com/PaddlePaddle/PaddleOCR)
- Uses [Jodit](https://xdsoft.net/jodit/) rich-text editor
- Uses [react-data-grid](https://github.com/adazzle/react-data-grid)

## Further Notes

This document was created by giving an AI access to the source code. The report was then edited and verified.