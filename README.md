# AI MCQs Global Quiz Engine v2

Host once on GitHub → serve anywhere via jsDelivr CDN.

## Repository structure

```
aimcq-engine/
├── aimcq.css          ← all styles (scoped to #aimcq-root-scope)
├── aimcq.js           ← full engine (initAimcqQuiz + loadAimcqFromDrive)
├── embed-snippet.html ← ready-to-paste code for any website
└── README.md
```

## 1 — Push to GitHub & create a release tag

```bash
git init
git add aimcq.css aimcq.js README.md
git commit -m "v2.0.0 — initial CDN release"
git remote add origin https://github.com/YOUR-USER/aimcq-engine.git
git push -u origin main
git tag v2.0.0
git push origin v2.0.0
```

## 2 — CDN URLs (available ~10 min after tagging)

```
https://cdn.jsdelivr.net/gh/YOUR-USER/aimcq-engine@2.0.0/aimcq.css
https://cdn.jsdelivr.net/gh/YOUR-USER/aimcq-engine@2.0.0/aimcq.js
```

> Always pin a version tag. Never use `@latest` in production.

## 3 — Embed on any website

See `embed-snippet.html` for the full copy-paste block.

**Head tags (once per site):**
```html
<link  rel="stylesheet" href="https://cdn.jsdelivr.net/npm/katex@0.16.22/dist/katex.min.css">
<link  rel="stylesheet" href="https://cdn.jsdelivr.net/gh/YOUR-USER/aimcq-engine@2.0.0/aimcq.css">
<script defer src="https://cdn.jsdelivr.net/npm/katex@0.16.22/dist/katex.min.js"></script>
<script defer src="https://cdn.jsdelivr.net/npm/katex@0.16.22/dist/contrib/auto-render.min.js"></script>
<script defer src="https://unpkg.com/smiles-drawer@2.0.1/dist/smiles-drawer.min.js"></script>
<script defer src="https://cdn.jsdelivr.net/gh/YOUR-USER/aimcq-engine@2.0.0/aimcq.js"></script>
```

**Optional global config (once per site, after the script tags above):**
```html
<script>
  window.AIMCQ_CONFIG = {
    appsScriptUrl: 'https://script.google.com/macros/s/YOUR_SCRIPT_ID/exec'
  };
</script>
```

**Quiz block — Method 1: Inline JSON**
```html
<div id="aimcq-quiz-1"></div>
<script>
document.addEventListener('DOMContentLoaded', function () {
  window.initAimcqQuiz('aimcq-quiz-1', /* quizDataJSON */, {
    title: "My Quiz", timer: 10,
    shuffle_questions: true, shuffle_options: true, quiz_questions: 10
  });
});
</script>
```

**Quiz block — Method 2: Single JSON from jsDelivr / CDN URL**
```html
<div id="aimcq-quiz-2"></div>
<script>
document.addEventListener('DOMContentLoaded', function () {
  window.loadAimcqFromDrive('aimcq-quiz-2', {
    jsonUrl: 'https://cdn.jsdelivr.net/gh/USER/REPO@TAG/path/quiz.json',
    settings: { title: "My Quiz", timer: 10, shuffle_questions: true, quiz_questions: 10 }
  });
});
</script>
```

**Quiz block — Method 3: Multiple JSON files merged (topic tabs + section headings)**
```html
<div id="aimcq-quiz-3"></div>
<script>
document.addEventListener('DOMContentLoaded', function () {
  window.loadAimcqFromDrive('aimcq-quiz-3', {
    jsonUrls: [
      { jsonUrl: 'https://cdn.jsdelivr.net/gh/USER/REPO@TAG/ch1.json', topic: 'Chapter 1' },
      { jsonUrl: 'https://cdn.jsdelivr.net/gh/USER/REPO@TAG/ch2.json', topic: 'Chapter 2' }
    ],
    settings: {
      title: "My Multi-Chapter Quiz", timer: 20,
      shuffle_questions: true, shuffle_options: true, quiz_questions: 20,
      topic_order: ['Chapter 1', 'Chapter 2']
    }
  });
});
</script>
```

> See `embed-snippet.html` for Method 4 (Google Drive via Apps Script proxy).

## Platforms confirmed compatible

- Blogger (original home)
- WordPress (paste in Custom HTML block)
- Plain HTML pages
- Any site that allows injecting `<script>` tags

## Updating the engine

1. Edit `aimcq.css` / `aimcq.js`
2. Commit and push a new tag (`v2.1.0`)
3. Update the version in your site's `<link>` and `<script>` src URLs
4. Old tagged versions remain available — existing embeds never break
