# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

BrainFest is a 100% static bilingual (FR/EN) educational quiz and memory-games site deployed to GitHub Pages at `brainfest.eu`. There is no build system, no npm, no framework — every page is a self-contained HTML file with embedded CSS and JS.

## Deployment

```bash
# Push to publish (deploys automatically via GitHub Pages)
# Auth token required — use this pattern:
git remote set-url origin https://vivelafrance756:[TOKEN]@github.com/vivelafrance756/brainfest.git
git push origin main
git remote set-url origin https://github.com/vivelafrance756/brainfest.git
```

The git repo lives inside `brainfest/` — all git commands must run from `/Users/catherinepiault/Documents/quiz-brainfest/brainfest/`.

## Architecture

### Page Structure

Every quiz chapter consists of 9 files following a strict naming pattern:

```
{chapter}.html                        ← intro/chapter hub
{chapter}-niveau{1-4}-questions.html  ← quiz engine
{chapter}-niveau{1-4}-fin.html        ← score/completion screen
```

Linear flow: intro → niveau1-questions → niveau1-fin → niveau2-questions → … → niveau4-fin.

Existing chapters: `magnesium`, `anti-inflammatoire`, `peau-et-foie`, `peau-microbiote`, `nutriments-et-peau`, `votre-peau`.

### Access Control

Every quiz page (questions + fin) guards entry with:
```javascript
if (!sessionStorage.getItem('bf_access')) { window.location.href = 'index.html'; }
```

Score is persisted per level via `sessionStorage.setItem('bf_score_{key}{level}', score)`.
Score keys by chapter: `mg` (magnésium), `ai` (anti-inflammatoire), `pf` (peau-et-foie), `pm` (peau-microbiote), `vp` (votre-peau).

### Bilingual System

- Global `lang` variable (default `'fr'`)
- Elements carry `data-fr="…" data-en="…"` attributes; `applyLang()` iterates them
- Language toggle button updates `lang` and calls `applyLang()` + re-renders questions
- Questions JS objects use `question_fr`/`question_en`, `text_fr`/`text_en`, `feedback_ok_fr`/`feedback_ok_en`, etc.

### Quiz Engine (questions pages)

Key variables: `currentIndex`, `score`, `selectedLetter`, `validated`, `reformulationAnswered`, `questionsLeft` (starts at 3 — AI questions budget).

Flow after validating a wrong answer:
1. Show `#feedback-block` (red) with `feedback_ko`
2. Show `#reformulation-block` — a Oui/Non question
3. `refoCorrectIsOui = q.reformulation_answer.toLowerCase().indexOf('oui') === 0`
4. On correct reformulation → `showPostFeedback()` after 1 s delay

After last question → `sessionStorage.setItem('bf_score_{key}', score)` → redirect to fin page.

AI chatbot endpoint: `https://brainfest-ai.c-piault.workers.dev` (POST with `{question, topic, language}`).

### Fin Pages

Levels 1–3: "Niveau N →" button + "Retour au chapitre" button.
Level 4: `#fin-congrats` italic amber quote + "Retour au chapitre" + "Accueil BrainFest" buttons. No "Niveau 5" button.

Score messages array (index 0–6): `['Continuez à explorer !', 'Continuez à explorer !', 'Pas mal, continuez !', 'Bien joué !', 'Très bien !', 'Excellent !', 'Parfait !']`

### index.html Cards

Active chapters use:
```html
<a href="{chapter}.html" class="card-link">
  <div class="card">…</div>
</a>
```

Inactive (coming soon) chapters use `<div class="card" onclick="soon()">` with a `<span class="card-soon-badge">`.

### Memory Games

`jeu-couleurs.html`, `jeu-notes.html`, `jeu-memoire-cerveau.html`, `memoire-botanique.html` — standalone games using Web Audio API and/or Speech Synthesis. Best scores in `localStorage`.

## Creating a New Chapter

1. Copy `peau-et-foie.html` → update title, image (`images/{chapter}.jpg`), level names/emojis, explore button links.
2. Copy `peau-et-foie-niveau1-questions.html` × 4 → update: score key, level emoji, back link, `sessionStorage.setItem` key, AI topic string, PubMed links, questions array.
3. Copy fin pages × 4 → update: score key, level emoji/name, next-level button link (level 4: replace with congrats + home buttons), chapter back link.
4. Activate card in `index.html` (replace `onclick="soon()"` div with `<a class="card-link">` wrapper).
5. Add chapter image to `images/`.

## Styling Conventions

CSS variables (defined in `:root` on every page):
`--indigo-dark: #2d3d4a` · `--indigo-mid: #1a2535` · `--amber: #d4923a` · `--cream: #f7f3ec` · `--sage: #6b9e78` · `--blue: #2a7fa8`

Quiz pages background: `#f5ede8`. Intro/hub pages background: `#fdf9f6`.
Fonts: Cormorant Garamond (headings) + DM Sans (body), loaded from Google Fonts.

All CSS and JS is embedded inline — no external `.css` or `.js` files.
