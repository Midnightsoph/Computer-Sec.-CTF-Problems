# FGCU Talon CTF

A lightweight, **browser-only** Capture the Flag (CTF) training platform for the Florida Gulf Coast University (FGCU) cybersecurity concentration. It runs as a single-page application with **no backend**—suitable for **GitHub Pages** or any static host.

## How to run locally

1. Open `index.html` in a modern browser (Chrome, Firefox, Safari, Edge).

   - Challenge data is embedded in `js/challenges.js` as well as `data/challenges.json`, so the app works when opened via `file://` (offline-friendly).
   - For development, you can also serve the folder over HTTP:

   ```bash
   cd fgcu-talon-ctf
   python3 -m http.server 8080
   ```

   Then visit `http://localhost:8080/`.

2. No install, no build step, no `npm`.

## Features

- **Vanilla HTML / CSS / JavaScript (ES modules)** — no frameworks.
- **Flags** are verified using **SHA-256** hashes (Web Crypto `crypto.subtle.digest`); plaintext flags are not stored in challenge data.
- **Progress** (solves, hints used, handle) persists in **localStorage**.
- **Have I Been Pwned** public [`/api/v3/breaches`](https://haveibeenpwned.com/api/v3/breaches) integration for the OSINT challenge (keyless). A **“Try it live”** button fetches JSON in the challenge view.
- **GitHub Users API** (`api-helpers.js`) is included as an optional recon-style fallback if you ever swap exercises—also keyless and rate-limited by IP.

## Why only keyless public APIs?

**API keys must never be embedded in frontend JavaScript.** Anyone can open DevTools, read the bundle, and exfiltrate secrets. For production apps that need authenticated third-party APIs, the correct pattern is a **server-side proxy** (e.g. Cloudflare Workers, Vercel Edge Functions, AWS Lambda) that holds the key and forwards **sanitized** requests.

This project is intentionally **frontend-only** for hosting simplicity, so it uses APIs that require **no authentication** (HIBP’s breach list is fully public). That choice is a **teaching moment**: students see a real integration *and* the security reasoning behind the architecture.

See also the comment in `js/api-helpers.js`.

## Challenge data

- Canonical JSON: `data/challenges.json` (flags as SHA-256 hex only).
- `js/challenges.js` mirrors that data for offline `file://` use—keep them in sync if you edit challenges.

## Assets

- `assets/forensics-challenge.txt` — simulated auth log for the forensics challenge.
- `assets/reverse-me.js` — client-side checker for the reverse challenge (educational; not a security boundary).
- `assets/stego-image.png` — placeholder bitmap for future steganography content (not used by the current six challenges).

## Future work

- **Serverless proxy** for any API that requires a key: one small function stores `API_KEY` in environment secrets, validates client input, and proxies to the vendor API.
- Optional **export/import** of progress as JSON for classroom demos.
- Additional challenges or a **steganography** track using `assets/stego-image.png` with real metadata tooling.

## Thesis / academic use

Designed as a senior thesis deliverable: emphasizes **platform UX**, **client-side verification patterns**, and **safe API usage** without implying client-only apps are appropriate for secret handling in production.
