# Movie App (React + Vite)

A learning-focused movie search and discovery app built with React and Vite. It fetches movies from TMDB and tracks trending searches using Appwrite. The project was created to practice core concepts of React.js, TailwindCSS, and consuming APIs.

Live demo: https://movie-dxfhcixbc-pandidharans-projects.vercel.app

## Why this project (Learning Objectives)
- React fundamentals: components, props, state, effects, conditional rendering.
- Hooks: useState, useEffect, and a debounced search pattern to reduce API calls.
- Data fetching: working with fetch, query params, headers, and error handling.
- Environment management: keeping secrets in `.env.local` and using Vite `import.meta.env`.
- TailwindCSS: building a clean UI with utility classes and responsive patterns.
- Backend-as-a-service (Appwrite): basic read/write with Databases, simple ranking (top searches).

## Architecture & Project Structure
High-level flow:
1) User types into the Search component.
2) Input changes update `searchTerm` in `App.jsx`.
3) A debounced effect triggers `fetchMovies(searchTerm)` after user pauses typing.
4) The app calls TMDB endpoints for either search results or popular discovery.
5) When there are search results, the first result is sent to Appwrite to update the trending counter.
6) A separate effect loads trending items from Appwrite for display.

Key files:
- `src/App.jsx`: Main page logic, state, effects, fetches, and rendering lists (All Movies, Trending).
- `src/assets/Components/Search.jsx`: Controlled input for the search term.
- `src/assets/Components/MovieCardComponent.jsx`: Renders a movie card (poster, title, etc.).
- `src/assets/Components/Spinner.jsx`: Simple loading spinner with accessible markup.
- `src/appwrite.js`: Appwrite client setup plus helper functions to update and read trending data.

Folder structure (simplified):
- `public/` — static images and icons.
- `src/` — application code (React components, styles, services).
- `README.md` — this guide.

## Features (Explained)
- Debounced Search: The app waits ~500ms after typing stops before calling the API to avoid spamming requests.
- Popular Movies (Discover): When search is empty, the app loads a default list of popular movies.
- Loading and Errors: `isLoading` toggles the Spinner, and user-friendly messages appear on failure.
- Trending Searches: Appwrite stores search terms and counts; the UI shows top results with poster previews.

## Environment Variables
Create a `.env.local` file in the project root using `.env.example` as a guide:
- `VITE_TMDB_API_KEY` — Your TMDB API Bearer token.
- `VITE_APPWRITE_PROJECT_ID` — Appwrite Project ID.
- `VITE_APPWRITE_DATABASE_ID` — Appwrite Database ID.
- `VITE_APPWRITE_COLLECTION_ID` — Appwrite Collection ID.

Important:
- `.env.local` is ignored by Git; never commit secrets.
- After editing env variables, stop and restart the dev server.

## TMDB API Integration
Endpoints used:
- Search: `GET /search/movie?query=<term>`
- Discover (popular): `GET /discover/movie` (sorted by popularity)

Implementation notes:
- All requests include an `Authorization: Bearer <token>` header from `VITE_TMDB_API_KEY`.
- `encodeURIComponent(query)` is used to safely pass search terms.
- The code guards against missing fields and sets empty arrays on failures.

## Styling with TailwindCSS
Approach:
- Utility-first classes keep components small and focused on logic.
- Common patterns: spacing (`mt-[]`, `p-[]`), flex/grid layout, text colors, gradients.
- Example: the Spinner uses classes for sizing and animation; headers use a gradient highlight span.

Tips:
- Compose small, readable class strings.
- Prefer conditional classes for states (loading, error) instead of inline styles.

## Appwrite (Trending Searches)
Client setup (in `src/appwrite.js`):
- Uses `{ Client, Databases, Query, ID }` from `appwrite`.
- Configured with `https://cloud.appwrite.io/v1` and the provided Project ID.

Functions:
- `updateSearchCount(searchTerm, movie)`: If a document with the term exists, increment `count`; otherwise create a new doc with `poster_url` and `movie_id`.
- `getTrendingMovies()`: Returns the top 5 documents ordered by `count` (desc).

Recommended collection attributes:
- `searchTerm` (string)
- `count` (integer)
- `movie_id` (string)
- `poster_url` (string)

Security notes:
- Configure Appwrite rules so only the app can write, and reads are limited as needed.
- Do not expose Appwrite keys on the client; this app uses public DB access constraints.

## Running the Project
- Install dependencies: `npm install`
- Start dev server: `npm run dev` (Vite dev server)
- Build for production: `npm run build`
- Preview production build: `npm run preview`

## Deployment (Vercel)
Steps used:
1) Push the repo to GitHub.
2) Import the project on Vercel and select the repo.
3) Set environment variables (`VITE_TMDB_API_KEY`, `VITE_APPWRITE_PROJECT_ID`, `VITE_APPWRITE_DATABASE_ID`, `VITE_APPWRITE_COLLECTION_ID`) in Vercel Project Settings.
4) Deploy. Vercel runs the build and serves the app.

Live demo:
- https://movie-dxfhcixbc-pandidharans-projects.vercel.app

## Troubleshooting
- Empty results or failed fetch: verify the TMDB Bearer token and CORS.
- Trending shows nothing: confirm Appwrite IDs and that documents exist; check rules and `Query.orderDesc('count')`.
- Env changes not taking effect: restart the dev server or redeploy on Vercel.

## Conclusion / Results
Through this project I consolidated the fundamentals of:
- React: composing components, lifting state, effects for data fetching, and conditional rendering.
- Hooks: practical debouncing to control API traffic and improve UX.
- APIs: authenticated requests with headers, handling responses, and guarding against errors.
- TailwindCSS: building a clean, responsive UI quickly with utility classes.
- Appwrite: using a managed backend to persist simple metrics and query sorted results.

The result is a small but complete React app that searches and lists movies, persists trending searches, and is production-deployed on Vercel with environment-based configuration.
