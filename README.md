# AI Mystery Room

A full-stack, AI-powered mystery investigation game. Cases are generated on the server with Google Gemini, validated before play, stored in MongoDB, and returned through spoiler-filtered public DTOs.

## Requirements

- Node.js 20+
- MongoDB (local or hosted)
- A Google Gemini API key

## Setup

1. Copy `.env.example` to `.env` and set `MONGODB_URI`, `GEMINI_API_KEY`, and a private `SESSION_SECRET`.
2. Install dependencies with `npm install`.
3. Start the application with `npm run dev`.
4. Open `http://localhost:5173`.

The client runs on Vite and proxies `/api` to Express on port 4000. To run separately, use `npm run dev:client` and `npm run dev:server`. Production client assets can be created with `npm run build`; run the API with `npm start`.

## Configuration

See `.env.example`. Gemini calls are made only by the server. MongoDB is required for durable cases and guest investigations. Missing provider configuration causes a clear generation error; the app does not silently substitute an invented demo mystery.

## Features

- Scenario, setting, category, difficulty, length, and evidence preference configuration.
- Server-side Gemini generation with strict structure, bounded repair, and consistency checks.
- MongoDB persistence of canonical cases and guest-owned investigation progress.
- Evidence room with suspect directory, timeline, notes, message records, witness interviews, reports, and clearly labeled image/CCTV reconstructions.
- Deterministic accusation check and solution reveal after the final accusation.
- Session-based guest ownership, rate limiting, and spoiler-safe DTOs.

All case content is fictional. Generated image/video capabilities are represented as labeled reconstructions or text descriptions unless a media provider is explicitly integrated.
