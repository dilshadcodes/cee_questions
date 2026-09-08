# MCQ Bank

A tiny static site for browsing chapter-wise MCQ sets. No build step, no backend —
just static files, deployable directly on GitHub Pages.

## Structure

```
mcq-app/
├── index.html          # Chapter list page
├── chapter.html        # Question viewer page
├── chapters.json        # Master list of chapters
├── style.css            # Shared styling
└── questions/
    └── wave-optics.json # One JSON file per chapter
```

## How it works

1. `index.html` fetches `chapters.json` and renders a clickable card for each chapter.
2. Clicking a chapter goes to `chapter.html?id=<chapter-id>`.
3. `chapter.html` looks up that `id` in `chapters.json` to find the right file under
   `questions/`, fetches it, and renders all questions with MathJax for LaTeX.

## Adding a new chapter

1. Create `questions/<your-chapter>.json` with this shape:

```json
{
  "chapter": "Kinematics",
  "questions": [
    {
      "number": 1,
      "question": "A body starts from rest... \\(v = u + at\\)",
      "options": [
        { "label": "a", "text": "10 m/s" },
        { "label": "b", "text": "20 m/s" },
        { "label": "c", "text": "30 m/s" },
        { "label": "d", "text": "40 m/s" }
      ]
    }
  ]
}
```

   Use `\\(` and `\\)` around inline LaTeX (double backslash, since it's inside a JSON string).

2. Add an entry to `chapters.json`:

```json
{
  "id": "kinematics",
  "name": "Kinematics",
  "file": "questions/kinematics.json",
  "count": 40
}
```

   - `id` — used in the URL (`chapter.html?id=kinematics`), keep it short and stable.
   - `name` — the human-readable name shown on the homepage and chapter page.
   - `file` — relative path to the question JSON.
   - `count` — optional, just shown as a hint under the chapter name.

That's it — no other files need to change.

## Deploying to GitHub Pages

1. Push this folder's contents to a GitHub repo (e.g. `mcq-bank`).
2. In the repo: **Settings → Pages → Source** → select the branch (usually `main`)
   and the root folder (`/`).
3. Save. GitHub will give you a URL like
   `https://<username>.github.io/mcq-bank/` — that's your `index.html`.

No server config needed since everything is `fetch()`-ed as static files over `https`.

## Local testing

Opening `index.html` directly by double-clicking it (`file://...`) will **not** work —
browsers block `fetch()` on local files. Instead, serve the folder locally, e.g.:

```bash
cd mcq-app
python3 -m http.server 8000
```

Then visit `http://localhost:8000` in your browser.
