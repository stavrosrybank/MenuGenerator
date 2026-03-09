# Menu Generator — Project Context

## What this project is
A personal weekly dinner planner SPA. Calls Claude API to generate 3 dinner suggestions per week, respects strict dietary rules, stores favourites/dislikes in localStorage, allows swapping individual meals. Deployed to Vercel.

## Tech Stack
- **Frontend**: Single `index.html` — plain HTML + embedded CSS + embedded JS. No build tools, no npm, no React.
- **Backend**: Vercel serverless function `api/claude.js` (Node.js CommonJS, no dependencies) — proxies Claude API calls to keep the API key off the browser.
- **Deployment**: Vercel (zero config)
- **Local dev**: `vercel dev` with `.env.local` containing `ANTHROPIC_API_KEY=sk-ant-...`

## Claude API
- Model: `claude-sonnet-4-6`
- `max_tokens`: 512 (week generation / swap)
- Endpoint proxied via `/api/claude`

## Dietary Rules (always in system prompt)
- No peanuts (allergy)
- Max 1 pasta dish and 1 rice dish per week
- Carb variety: vary across the week (couscous, potatoes, lentils, polenta, bread, etc.)
- ~1x fish or meat per 6 meals; ~1x vegan per 6 meals; otherwise vegetarian
- Max 30–40 min cooking time
- Seasonal ingredients for current month in Germany
- Central European / Mediterranean style; Indian, Israeli, Asian also welcome
- Nutritional goals: protein in every meal, Vitamin C at least 2x, Omega-3 at least 1x, iron + zinc across the week

## Language Rules
- UI text, buttons, labels, descriptions: **English**
- Dish names: native cuisine language (e.g. "Shakshuka", "Bibimbap") with English description
- Tags: English pills (vegetarian, vegan, fish, meat)

## Data Model

### Meal (returned by Claude) — lean, no ingredients or steps
```json
{
  "name": "Dish name",
  "description": "One sentence in English (max 20 words)",
  "nutrition": "Main nutritional benefit in English (max 10 words)",
  "cookTime": "25 Min",
  "carbType": "couscous",
  "tags": ["vegetarian"]
}
```

### localStorage Keys
| Key | Content |
|---|---|
| `mg_meals` | Current week's meals (Meal[]) |
| `mg_favourites` | Saved favourites ({ name, description, tags }[]) |
| `mg_disliked` | Never-show dish names (string[]) |
| `mg_last_week` | Comma-joined names from previous week |

## Hardcoded values
- `mealsPerWeek = 3` (constant, not user-configurable)
- No settings page, no cuisine preference UI

## File Structure
```
MenuGenerator/
  index.html       — full app
  api/claude.js    — Vercel serverless function
  CLAUDE.md        — this file
  README.md        — deployment instructions
  .env.local       — local dev only (gitignored)
  .gitignore
```
