# Typing Trainer

A single-page **keyboard typing trainer** built with **HTML, CSS, and vanilla JavaScript**. Open `index.html` in a browser—**no build step or local server is required**.

The UI is mostly in **Russian**; this document describes behavior in English.

## Quick start

1. Clone or copy the project folder.
2. Double-click `index.html` (or open it via **File → Open** in your browser).

For best results with live Wikipedia text, use a **recent Chromium, Firefox, or Safari** build.

## Features

- **Live statistics** — characters per minute (CPM), words per minute (WPM, 5 characters = 1 word on correct keystrokes), accuracy, elapsed time, error count.
- **Reference line** — large, color-coded preview: pending, current, correct, and error feedback.
- **Strict input** — wrong characters are **not** appended; you must type the correct key to continue. A short error sound and a red flash mark mistakes.
- **Key feedback** — subtle “click” via the **Web Audio API** when a character is accepted.
- **Results history** — a side table records each **completed** run (time, mode, CPM, WPM, accuracy, duration, errors). New rows appear at the top.
- **Mode toolbar** — one button stays visually selected; the footer hint text switches per mode.

## Training modes

| Button (UI) | Description |
|-------------|-------------|
| **Текст** | Loads a random excerpt from [Simple English Wikipedia](https://simple.wikipedia.org) via the REST API (`/api/rest_v1/page/random/summary`). Snippets are trimmed (~420 chars) and **rejected** if any Unicode letter is not ASCII `A–Z` / `a–z` (digits and punctuation are allowed). Up to **20** random articles may be tried before giving up. |
| **Цифры** | Fixed two-digit pair drills: **56↔65**, **78↔87**, **12↔21**, **90↔09**, **34↔43** — each pair alternates **10** times, space-separated (100 tokens total). |
| **Числа** | **36** blocks of **4** random digits, separated by spaces. |
| **Биграммы** | **80** random bigrams from a pool of **40** common English letter pairs, space-separated. |
| **Триммы** | **40** random fragments from a pool of **40** common English trigram-like chunks (mostly 3–4 letters), space-separated. |
| **Связки** | **40** random fragments from a pool of **40** longer English letter clusters (rolls / morphemes), space-separated. |

Tuning knobs in `index.html` (inside the main `<script>` block) include: `MAX_WIKI_ATTEMPTS`, `FALLBACK`, `DIGITS_PAIR_SECTIONS`, `DIGITS_PAIR_REPEAT`, `NUMBERS_BLOCK_LEN`, `NUMBERS_COUNT`, the `BIGRAMS` / `TRIMMS` / `CHUNKS` pools plus their `*_COUNT` and `*_SEP` values, and `WIKI_SUMMARY` if you point the app at another compatible API.

## Wikipedia, CORS, and offline behavior

The app uses `fetch(..., { mode: "cors" })`. Wikimedia’s API typically sends **`Access-Control-Allow-Origin: *`**, so cross-origin requests work from normal `http(s)` pages.

If the network fails, CORS blocks the request (e.g. some **`file://`** setups with a `null` origin), or no suitable snippet is found after **20** attempts, the app falls back to **built-in English paragraphs** (`FALLBACK` in the script) and shows a short message above the reference text.

## Technical notes

- **Enter** in the typing area is ignored (newlines are not part of the exercise).
- The textarea uses **`resize: vertical`** so width stays aligned with the layout.
- **Unicode property escapes** (`\p{L}`) are used to filter Wikipedia snippets; use a browser that supports **ES2018+ RegExp** features.
- **No persistence** — refreshing the page clears the on-screen history table.

## File layout

```
typing-trainer/    # project root (name may differ)
├── index.html     # Full app: structure, styles, logic
└── README.md      # This file
```

## License

No license is specified in the repository; treat as private / all rights reserved unless you add one.
