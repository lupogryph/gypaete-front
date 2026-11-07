# Chalet le Gypaète

A bilingual Vue 3 showcase for the chalet. The page loads chalet details from one JSON endpoint and supports French and English.

## Run locally

```sh
npm install
cp .env.example .env
```

Set `VITE_CHALET_API_URL` in `.env` to the URL that returns the chalet JSON, then run:

```sh
npm run dev
```

The endpoint must allow browser requests from the frontend origin (CORS). If no URL is configured, the app requests `/public`.
