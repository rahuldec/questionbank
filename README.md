# Examsmith

A single-page, standalone web app that reads a PDF and generates a question bank from it — multiple choice, fill-in-the-blank, short/long answer, assertion–reason and numerical/applied questions — grounded strictly in that PDF's own content, with an answer key that cites a verbatim source quote for every question.

No backend, no build step, no server: it's one `index.html` file. PDF text is extracted client-side with pdf.js, and question generation calls the Anthropic API directly from your browser using your own API key.

## Usage

1. Open `index.html` — locally (double-click it) or host it anywhere static (GitHub Pages, Netlify, `python -m http.server`, ...).
2. Paste an [Anthropic API key](https://console.anthropic.com/settings/keys) and click **Save key**. It's stored only in your browser's `localStorage` and sent directly to `api.anthropic.com` — this repo has no server, so nothing passes through anywhere else.
3. Drop in a PDF, pick question types/counts and a difficulty, and click **Generate**.
4. Print/save as PDF, or download a plain-text version.

## How it works

1. **Extraction** — pdf.js (loaded from cdnjs) reads the PDF's text entirely in-browser.
2. **Prompting** — the extracted text (capped at 60,000 characters) plus your settings are assembled into one prompt that instructs the model to answer using *only* that text, and to attach a verbatim source quote to every question.
3. **Generation** — a `fetch` call goes straight to `https://api.anthropic.com/v1/messages` with your API key, using the `anthropic-dangerous-direct-browser-access` header that Anthropic provides for exactly this kind of client-only prototype. The reply is parsed as JSON (tolerant of stray text/code fences around it) and rendered as the answer sheet.

## Notes

- Calling the Anthropic API straight from the browser means your API key is present in this page's JS runtime — fine for personal/local use, but don't deploy this publicly with a key embedded, and be aware anyone with access to the page (or a browser extension with page access) can read a key you've saved in localStorage.
- Model tier (Fast / Balanced / Powerful) maps to Haiku / Sonnet / Opus.
