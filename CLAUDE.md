# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## About

A React expense tracker app used as a teaching project in a Claude Code course. It intentionally ships with a bug, poor UI, and messy code.

## Commands

```bash
npm install       # install dependencies (requires Node.js 20.19+ or 22.12+)
npm run dev       # start dev server at http://localhost:5173
npm run build     # production build
npm run preview   # preview production build
npm run lint      # run ESLint
```

## Architecture

The entire app lives in a single component: `src/App.jsx`. There is no routing, no context, no separate components — all state and rendering are in `App`.

**Known bug**: `amount` is stored as a string in state (from the form input). The `totalIncome` and `totalExpenses` reducers use `+` on strings, so they concatenate instead of summing numerically. Fix: parse `amount` to a number when creating a new transaction or when reading it in the reducers.

**State shape** (all in `App`):
- `transactions` — array of `{ id, description, amount, type, category, date }`
- Form fields: `description`, `amount`, `type`, `category`
- Filter fields: `filterType`, `filterCategory`

**Data flow**: transactions are filtered client-side on render using `filterType` and `filterCategory`; there is no persistence (state resets on refresh).
