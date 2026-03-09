# Menu Generator

A personal weekly dinner planner powered by Claude AI.

## Features
- Generates N dinner suggestions per week (configurable, default 3)
- Respects dietary rules (no peanuts, carb variety, vegetarian-focused)
- Seasonal German ingredients
- Swap individual meals
- Save favourites and mark dislikes
- Consolidated shopping list
- Cuisine preferences (Greek, Italian, German, Korean, and more)

## Local Development

### Prerequisites
- [Vercel CLI](https://vercel.com/cli): `npm i -g vercel`

### Setup
1. Clone the repo
2. Create `.env.local` in the project root:
   ```
   ANTHROPIC_API_KEY=sk-ant-your-key-here
   ```
3. Run:
   ```bash
   vercel dev
   ```
4. Open `http://localhost:3000`

> **Note**: `.env.local` is gitignored and never committed.

## Vercel Production Deployment

1. Push to GitHub
2. Connect the repo in [Vercel dashboard](https://vercel.com/new)
3. Add environment variable: `ANTHROPIC_API_KEY = sk-ant-your-key-here`
4. Deploy — Vercel auto-deploys on every push to main

## Usage

- Click **New Week** to generate this week's meals
- Click a meal card to expand ingredients and cooking steps
- Use **Swap** to replace a single meal
- Use **★ Save** to add to favourites
- Use **✕ Never again** to permanently exclude a dish
- Click **Generate shopping list** to get a consolidated grocery list
- Click **⚙** to configure cuisine preferences and meals per week
- Click **ℹ** to see current settings, dietary rules, favourites, and dislikes

## Tech Stack
- Plain HTML/CSS/JS (no build tools)
- Vercel serverless function for API proxy
- Claude API (`claude-sonnet-4-6`)
