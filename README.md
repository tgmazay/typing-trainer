# Typing Trainer

A single-page **keyboard typing trainer** built with **HTML, CSS, and vanilla JavaScript**. Open `index.html` in a browser—**no build step or local server is required**.

The interface is available in **English** and **Russian**. **English is the default**; use the **language** dropdown in the header to switch (values are locale keys: `en`, `ru`). Choice is stored in **`localStorage`** under `typing-trainer-lang`. This document describes behavior in English.

## Quick start

1. Clone or copy the project folder.
2. Double-click `index.html` (or open it via **File → Open** in your browser).

For best results with live Wikipedia text, use a **recent Chromium, Firefox, or Safari** build.

## Features

- **Localization** — UI strings, mode hints, exercise titles, and sound preset labels (where translated) live in a **JSON blob** embedded in `index.html` (`<script type="application/json" id="i18n-data">`). At runtime the app parses it once and reads the active locale by key (`en` / `ru`, …).
- **Live statistics** — characters per minute (CPM), words per minute (WPM = rounded CPM ÷ 5, treating 5 correct characters as one “word”), accuracy, elapsed time, error count. The speed timer starts when the **first correct** character is accepted; CPM/WPM use only **correct** characters in the numerator.
- **Reference line** — large, color-coded preview: pending, current, correct, and error feedback.
- **Strict input** — wrong characters are **not** appended; you must type the correct key to continue. A short error sound and a red flash mark mistakes.
- **Key feedback** — **Web Audio** profiles selectable in the footer: mechanical-style layers (tone + noise + thump), **IBM Model M / F** (extra metal ring), and **typewriter** presets (impact + resonance + mechanics + paper; line bell every 70 accepted characters; **Enter** plays a carriage-return sound in typewriter mode while still blocking newlines in the text). Choice is stored in **`localStorage`** (`typing-trainer-key-sound`) for that browser.
- **Results history** — a side table records each **completed** run (time, mode, CPM, WPM, accuracy, duration, errors). New rows appear at the top.
- **Mode toolbar** — one button stays visually selected; the hint under the toolbar switches per mode.

## Training modes

Labels below match the **English** UI; Russian uses the same modes with translated names.

| Button (EN / RU) | Description |
|------------------|-------------|
| **Text** / **Текст** | Loads a random excerpt from [Simple English Wikipedia](https://simple.wikipedia.org) via the REST API (`/api/rest_v1/page/random/summary`). Snippets are trimmed (~420 chars) and **rejected** if any Unicode letter is not ASCII `A–Z` / `a–z` (digits and punctuation are allowed). Up to **20** random articles may be tried before giving up. |
| **Digits** / **Цифры** | Fixed two-digit pair drills: **56↔65**, **78↔87**, **12↔21**, **90↔09**, **34↔43** — each pair alternates **10** times, space-separated (100 tokens total). |
| **Numbers** / **Числа** | **36** blocks of **4** random digits, separated by spaces. |
| **Bigrams** / **Биграммы** | **80** random bigrams from a pool of **40** common English letter pairs, space-separated. |
| **Trims** / **Триммы** | **40** random fragments from a pool of **40** common English trigram-like chunks (mostly 3–4 letters), space-separated. |
| **Chunks** / **Связки** | **40** random fragments from a pool of **40** longer English letter clusters (rolls / morphemes), space-separated. |
| **Words** / **Слова** | **40** random English words per run, drawn from the **`WORDS_TOP300` array** defined in `index.html` (full word list lives only there). Words are space-separated; entries may include apostrophes (**don’t**, **won’t**, etc.). |

Tuning knobs in `index.html` (inside the main `<script>` block) include: `MAX_WIKI_ATTEMPTS`, `FALLBACK`, `DIGITS_PAIR_SECTIONS`, `DIGITS_PAIR_REPEAT`, `NUMBERS_BLOCK_LEN`, `NUMBERS_COUNT`, the `BIGRAMS` / `TRIMMS` / `CHUNKS` / `WORDS_TOP300` pools plus their `*_COUNT` and `*_SEP` values (where applicable), keyboard/typewriter preset parameters on the `KeyboardSound` / `TypewriterSound` prototypes, and `WIKI_SUMMARY` if you point the app at another compatible API. To change UI copy or add a locale, edit the embedded **`i18n-data`** JSON (top-level keys are the locale codes used by the language `<select>`). To change the vocabulary for **Words**, edit the `WORDS_TOP300` array in `index.html`.

## Wikipedia, CORS, and offline behavior

The app uses `fetch(..., { mode: "cors" })`. Wikimedia’s API typically sends **`Access-Control-Allow-Origin: *`**, so cross-origin requests work from normal `http(s)` pages.

If the network fails, CORS blocks the request (e.g. some **`file://`** setups with a `null` origin), or no suitable snippet is found after **20** attempts, the app falls back to **built-in English paragraphs** (`FALLBACK` in the script) and shows a short message above the reference text (localized).

## Technical notes

- **Enter** in the typing area does not insert a newline (newlines are not part of the exercise); in **typewriter** sound mode it still triggers the carriage-return sample.
- The textarea uses **`resize: vertical`** so width stays aligned with the layout.
- **Unicode property escapes** (`\p{L}`) are used to filter Wikipedia snippets; use a browser that supports **ES2018+ RegExp** features.
- **History** is not persisted across reloads; **language**, **key sound**, and related preferences are persisted via **`localStorage`**.

## File layout

```
typing-trainer/    # project root (name may differ)
├── index.html     # Full app: structure, styles, logic, embedded i18n JSON
└── README.md      # This file
```

## License

No license is specified in the repository; treat as private / all rights reserved unless you add one.
