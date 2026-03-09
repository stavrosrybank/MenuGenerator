# Menu Generator — Project Context

## What this project is
A personal weekly dinner planner SPA. Calls Claude API to generate N dinner suggestions per week, respects strict dietary rules, stores favourites/dislikes in localStorage, allows swapping individual meals, and generates a consolidated shopping list. Deployed to Vercel.

## Tech Stack
- **Frontend**: Single `index.html` — plain HTML + embedded CSS + embedded JS. No build tools, no npm, no React.
- **Backend**: Vercel serverless function `api/claude.js` (Node.js, no dependencies) — proxies Claude API calls to keep the API key off the browser.
- **Deployment**: Vercel (zero config)
- **Local dev**: `vercel dev` with `.env.local` containing `ANTHROPIC_API_KEY=sk-ant-...`

## Claude API
- Model: `claude-sonnet-4-6`
- `max_tokens`: 2048 (week generation / swap), 1024 (shopping list)
- Endpoint proxied via `/api/claude`

## Dietary Rules (always in system prompt)
- No peanuts (allergy)
- Max 1 pasta dish and 1 rice dish per week
- Carb variety: vary across the week (couscous, potatoes, lentils, polenta, bread, etc.)
- ~1x fish or meat every 2 weeks, ~1x vegan every 2 weeks; otherwise vegetarian
- Max 30–40 min cooking time
- Seasonal ingredients for current month in Germany
- Central European / Mediterranean style (overridden by cuisine preferences)
- Nutritional goals: protein in every meal, Vitamin C at least 2x, Omega-3 at least 1x, iron + zinc across the week

## Language Rules
- UI text, buttons, labels, descriptions, steps: **English**
- Dish names: native cuisine language (e.g. "Shakshuka", "Bibimbap") with English description
- Ingredients: items in English or German (natural), units in **German** (g, ml, EL, TL, Stück, Prise, Bund)
- Tags: English pills (vegetarian, vegan, fish, meat)

## Data Model

### Meal (returned by Claude)
```json
{
  "name": "Dish name",
  "description": "One sentence in English (max 20 words)",
  "nutrition": "Main nutritional benefit in English (max 10 words)",
  "cookTime": "25 Min",
  "carbType": "couscous",
  "tags": ["vegetarian"],
  "ingredients": [{ "amount": "200", "unit": "g", "item": "Kartoffeln" }],
  "steps": ["Step 1...", "Step 2..."]
}
```

### localStorage Keys
| Key | Content |
|---|---|
| `mg_meals` | Current week's meals (Meal[]) |
| `mg_favourites` | Saved favourites ({ name, description, tags }[]) |
| `mg_disliked` | Never-show dish names (string[]) |
| `mg_last_week` | Comma-joined names from previous week |
| `mg_prefs` | User preferences ({ cuisines: string[], mealsPerWeek: number }) |

## Preferences (`mg_prefs`)
```json
{
  "cuisines": ["greek","italian","german","eastern_european","spanish","israeli","indian","korean","vietnamese"],
  "mealsPerWeek": 3
}
```
All cuisines selected by default. `mealsPerWeek` default: 3, range 1–7.

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
