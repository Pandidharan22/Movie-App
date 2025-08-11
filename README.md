# Movie App (React + Vite)

A simple movie search and discovery app built with React and Vite. It fetches movies from TMDB and tracks trending searches using Appwrite.

## Features
- Search movies with debounced input
- Discover popular movies
- Loading state with spinner and error handling
- Trending movies list powered by Appwrite (top searches)

## Tech Stack
- React (Vite)
- Appwrite Web SDK
- Fetch API

## Prerequisites
- Node.js 18+ and npm
- TMDB account + API key
- Appwrite project, database, and collection

## Environment Variables
Create a `.env.local` file in the project root using `.env.example` as a reference:

- `VITE_TMDB_API_KEY` — Your TMDB API Bearer token
- `VITE_APPWRITE_PROJECT_ID` — Appwrite Project ID
- `VITE_APPWRITE_DATABASE_ID` — Appwrite Database ID
- `VITE_APPWRITE_COLLECTION_ID` — Appwrite Collection ID

Note: `.env.local` is ignored by Git.

## Appwrite Setup (for Trending)
Create a collection with at least these attributes:
- `searchTerm` (string)
- `count` (integer)
- `movie_id` (string)
- `poster_url` (string)

The app increments `count` for an existing `searchTerm`, or creates a new document when a term is searched.

## Scripts
- Install: `npm install`
- Dev server: `npm run dev`
- Build: `npm run build`
- Preview build: `npm run preview`

## Notes
- After changing env variables, restart the dev server.
- If TMDB requests fail, verify the Bearer token and that it’s exposed as `VITE_TMDB_API_KEY`.
- Ensure Appwrite endpoint is `https://cloud.appwrite.io/v1` and the provided IDs are correct.
