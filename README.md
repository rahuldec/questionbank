# Examsmith

A single-page tool that reads a PDF (extracted entirely in your browser) and generates a question bank — multiple choice, fill-in-the-blank, short/long answer, assertion–reason and numerical/applied questions — grounded strictly in that PDF's own content, with an answer key that cites a verbatim source quote for every question.

## Usage

Open `index.html`. It's a self-contained page published as a Claude Artifact, so it needs to run inside a Claude.ai viewer that grants the `sample` and `downloads` runtime capabilities — it won't generate questions when opened as a plain static file outside that context.

Live version: https://claude.ai/artifact/3L1CtiSa7JM5k9ENhqhNPC

## How it works

1. Drop in a PDF — text is extracted client-side with pdf.js, nothing is uploaded anywhere.
2. Pick how many questions of each type, and a difficulty.
3. Generate — the extracted text plus instructions are sent to Claude (via the artifact's `sample` capability, on the viewer's own account), which returns a structured question bank as JSON.
4. Print/save as PDF, or download a plain-text version.
