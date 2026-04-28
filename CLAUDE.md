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

There is no routing, no context, and no persistence (state resets on refresh). The component tree is:

```
App
├── Summary
├── TransactionForm
└── TransactionList
```

**`App`** — holds the single source of truth: the `transactions` array (`{ id, description, amount, type, category, date }`). Passes `transactions` down to all children and an `onAdd` callback to `TransactionForm`.

**`Summary`** — receives `transactions` and derives `totalIncome`, `totalExpenses`, and `balance` internally.

**`TransactionForm`** — owns its own form state (`description`, `amount`, `type`, `category`). Calls `onAdd` with the new transaction object on submit. `amount` is stored as a string in form state and parsed to a float via `parseFloat` before being passed up.

**`TransactionList`** — receives `transactions` and owns filter state (`filterType`, `filterCategory`). Filters client-side on render.

The `categories` constant is duplicated in `TransactionForm` and `TransactionList` — a shared constants file does not yet exist.
