# ◈ Regex Sentinel

> A recursive regex parser and real-time validator with a telemetry-style visual interface.

Regex Sentinel breaks any regular expression into a **Token Tree** — anchors, quantifiers, character classes, groups, lookaheads — and renders each token as a human-readable card in a scrollable **Flight Plan**. Type a test string and every match is highlighted in real time with a cycling colour palette. Hover a card to illuminate the exact characters it maps to in the pattern, and vice versa.

---

## Features

### Parser Engine
- **Recursive descent parser** — full grammar: alternation → sequence → token → atom → quantifier
- **14 token types** with precise `start/end` offsets powering all visual sync
- Handles every group variant: capturing `()`, non-capturing `(?:)`, named `(?<name>)`, positive/negative lookahead `(?=)` `(?!)`, positive/negative lookbehind `(?<=)` `(?<!)`
- Quantifier parsing: `*` `+` `?` `{n}` `{n,}` `{n,m}` — greedy and lazy (`?` suffix)
- Character classes with ranges, negation, and escape sequences
- Escape sequences: `\d \w \s \b \n \t \r \x## \u#### \uFFFF` and back-references `\1`–`\9`
- Native `RegExp` pre-validation — parse errors surface as an inline red tooltip with the exact message

### Flight Plan (Token Tree)
- Each token renders as a **Translation Card** with type badge, raw pattern fragment, human label, and full description
- Quantifier cards embed their target atom as an indented child rail ("Applies to:")
- Group cards embed their full contents tree recursively ("Contents:")
- Alternation cards split into labelled branches ("Branch 1", "Branch 2" …)
- Flag annotations appear on affected cards (e.g. `m: per-line` on Anchor cards, `s: matches \n` on Wildcard cards)
- Vertical flight-line connects all top-level cards

### Two-Way Hover Sync
- **Card → Pattern**: hovering a Flight Plan card highlights the exact character range it covers in the Syntax View
- **Pattern → Card**: hovering a coloured character in the Syntax View highlights and scrolls to its owning card
- Click either side to create a persistent lock-selection; click again to release

### Live Validator
- Syntax View renders the pattern with **per-character token colouring** — each character is painted the colour of its innermost token type
- Test String input highlights all matches in real time with a **5-colour cycling palette** (cyan → orange → green → purple → yellow)
- Match counter badge updates on every keystroke
- Capture Group table shows every match row with numbered and named group columns and per-match colour coding

### Flag Chips
Toggle `g` `i` `m` `s` independently with live re-evaluation:

| Flag | Effect |
|------|--------|
| `g`  | Global — find all matches, not just the first |
| `i`  | Ignore case — case-insensitive matching |
| `m`  | Multiline — `^` and `$` match line boundaries |
| `s`  | DotAll — `.` also matches newline characters |

### Preset Library
Six built-in patterns to explore the parser immediately:

| Preset | Pattern |
|--------|---------|
| Email Address | `^[\w.-]+@[\w.-]+\.\w{2,}$` |
| IPv4 Address | `^(\d{1,3}\.){3}\d{1,3}$` |
| UUID v4 | `[0-9a-f]{8}-([0-9a-f]{4}-){3}[0-9a-f]{12}` |
| URL (http/s) | `https?:\/\/[\w.-]+(\/[\w.\/-]*)?` |
| ISO 8601 Date | `\d{4}-\d{2}-\d{2}` |
| Hex Color | `#(?:[0-9a-fA-F]{3}){1,2}\b` |

### UI
- Dark / Light mode toggle
- Copy regex button with flash confirmation
- Clear all button
- JetBrains Mono monospaced font throughout
- Subtle cyan grid texture in dark mode
- Glowing borders on active matches and selected cards

---

## Tech Stack

| Concern | Approach |
|---------|----------|
| Parser | Vanilla JS recursive descent — zero dependencies |
| Styling | Custom CSS variables + Tailwind CDN utility resets |
| Font | JetBrains Mono via Google Fonts |
| Deployment | Single `index.html` — no build step, no bundler, no framework |

---

## Getting Started

### Run locally
Just open the file in any modern browser:
```bash
open index.html
```
No install, no server, no dependencies to fetch beyond the CDN links in `<head>`.

### Deploy to GitHub Pages
1. Push `index.html` to a public GitHub repository
2. Go to **Settings → Pages → Deploy from branch → main / root**
3. Live at `https://your-username.github.io/repo-name`

### Deploy to Vercel
1. Import the GitHub repository in the Vercel dashboard
2. Framework preset: **Other** (no build command, no output directory)
3. Click **Deploy** — live at `https://your-project.vercel.app`

---

## Project Structure

```
index.html          ← entire application (parser + UI + styles in one file)
README.md
```

Everything lives in `index.html` — the `<style>` block contains all CSS, and the single `<script>` block at the bottom contains the full parser, renderer, match engine, and event handlers.

---

## Token Type Reference

| Token | Colour | Examples |
|-------|--------|---------|
| Anchor | Purple | `^` `$` |
| Literal | Grey | `a` `B` `3` |
| Character Class | Blue | `[a-z]` `[^0-9]` |
| Escape | Yellow | `\d` `\w` `\s` `\b` |
| Wildcard | Green | `.` |
| Quantifier | Orange | `*` `+` `{3,5}` |
| Capture Group | Green | `(abc)` |
| Non-Capturing Group | Light Blue | `(?:abc)` |
| Named Group | Yellow | `(?<name>abc)` |
| Positive Lookahead | Pink | `(?=abc)` |
| Negative Lookahead | Red | `(?!abc)` |
| Positive Lookbehind | Orange | `(?<=abc)` |
| Negative Lookbehind | Red-Orange | `(?<!abc)` |
| Alternation | Purple | `a\|b\|c` |

---

## License

MIT — do whatever you like with it.
