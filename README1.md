# ClauseGuard

An informational rental-agreement reviewer. Upload a PDF or DOCX, see it split into clauses with page references, then (optionally) get a Claude-powered review and ask questions about the contract.

**Not legal advice.** Risk labels are preliminary indicators, not legal conclusions.

## Files

- `index.html` – the whole app: HTML, CSS and JavaScript in one file. (Same content as `clauseguard-landing.html`; GitHub Pages needs the name `index.html`.)
- `clauseguard-landing.html` – identical copy of the app under its original name.
- `README.md` – this file.

## Run it

1. Open `index.html` in a browser (internet needed, see below).
2. Choose a PDF or DOCX in the **Try document extraction** section.

Extraction and clause splitting work anywhere. The Claude-powered features work only when the page is opened on claude.ai (see below).

External scripts (pinned versions, loaded from cdnjs): `pdf.js 3.11.174` (PDF text), `mammoth 1.6.0` (DOCX text). Font: Plus Jakarta Sans from Google Fonts, with system fallbacks.

## Publish on GitHub Pages (opens the site directly)

A repository page on github.com always shows the README. To open the site itself, use GitHub Pages:

1. Put `index.html` in the root of the repository (next to `README.md`) and push.
2. In the repository, go to **Settings → Pages**.
3. Under **Build and deployment**, set **Source** to **Deploy from a branch**, pick your main branch and the **/ (root)** folder, then save.
4. After a minute the site is live at `https://<your-username>.github.io/<repository-name>/`. Share that link instead of the repository link.

On GitHub Pages the upload, extraction and clause splitting work. The Claude-powered features do not, because they need the claude.ai runtime. They show a "not available" message until you connect your own backend (see below).

## What works

| Feature | How it works |
|---|---|
| Upload and validation | PDF or DOCX, up to 15 MB. Shows name, size and progress. Clear errors for wrong type, empty, corrupted, password-protected or scanned files. |
| Text extraction | In the browser. PDFs are read page by page so each clause keeps its page number. DOCX has no page numbers. |
| Clause splitting | Rule-based: "Clause 3", "Section 2", "4." numbering and ALL-CAPS headings. |
| Category tags | Keyword matching (rent, deposit, notice period, termination, lock-in, maintenance, penalty, renewal, dispute resolution, other). Not AI. |
| Explain each clause | Claude gives a plain-English explanation, a Low/Medium/High label, a reason and a confidence level per clause. |
| Key terms table | Claude extracts rent, deposit, notice, duration, due date, late fee, deposit return, lock-in and renewal. Each value is shown only if its quoted wording is found in the cited clause and its figures appear in that quote. |
| Risk findings | Claude checks a 14-topic checklist across the whole contract. Levels: Low, Medium, Medium-High, High. Each finding quotes the contract wording, gives a plain explanation and a suggested question to ask. Findings whose quote is not found in the cited clause are discarded. |
| Legal reference library | 14 short paraphrases (Rajasthan Rent Control Act, 2001; Indian Contract Act s.74; Transfer of Property Act s.106 and s.107), each with a source link. Findings can show a "Legal reference" box that is clearly marked as external to the contract. |
| Ask your contract | The 6 most relevant clauses (keyword scoring) are sent to Claude. Answers need a valid citation from those clauses, otherwise the page shows: "This information could not be found in the uploaded contract." |

## AI and permission

The AI features use the Claude runtime available to pages published on claude.ai. The first time you use one, you are asked to allow permission. If you decline, or open the file anywhere else, those features show a clear message and nothing is sent.

Only clause text is sent, and only when you press a review button or ask a question. The uploaded file itself is never uploaded.

## Using your own backend

To run outside claude.ai, replace the `claude.use('sample')` calls (search the file for `sample.json`) with a request to a small server of yours that calls the Anthropic API. Keep the API key on the server, for example in an `ANTHROPIC_API_KEY` environment variable, and never in the page.

## Legal reference library

Entries are short paraphrases, checked against the linked sources on 24 October 2026:

- Rajasthan Rent Control Act, 2001: s.1, 3, 5, 7, 9, 11, 18, 22-B, 22-C, 22-F, 23, 24, 25 (India Code copy with amendments up to Act 33 of 2017; later changes may not be shown).
- Indian Contract Act, 1872: s.74.
- Transfer of Property Act, 1882: s.106, s.107.

Limits:

- Rajasthan and three national laws only. No other states and no case law.
- In Jaipur, Chapters II and III of the Rajasthan Act (rent revision and eviction grounds) do not apply above a rent threshold. The copy used omits the lead-in words, so check the official text.
- The library is small and may be incomplete or out of date. Confirm anything important at the source.

## Current limitations

- Scanned PDFs (no OCR) are not supported.
- Clause splitting can be wrong for contracts with unusual numbering.
- Very long clauses are shortened before review (700 characters for per-clause explanations, 3,000 for the full review). Only the first 40 clauses with enough text get a per-clause explanation.
- Keyword retrieval for Q&A can miss a clause that uses different words than the question.
- Draft comparison, negotiation wording beyond the per-finding suggestion, vector search and accounts are not built.
- Not tested on a wide range of real contracts.

## Privacy

- The file is read in your browser and is not uploaded.
- Clause text goes to Claude only when you run a review or ask a question.
- Nothing is stored by the page.
