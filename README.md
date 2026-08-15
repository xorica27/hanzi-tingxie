# 汉字 Writing Quiz

A single-file web app for practising Chinese character handwriting. Type the words you
want to drill, then write each character stroke by stroke and get graded on stroke order.

**Live:** https://hanzi-tingxie.vercel.app

## Two modes

| Mode | What you see | What you do |
|---|---|---|
| **描红** (trace) | The character outline | Trace over it |
| **听写** (dictation) | An empty box | Listen to the word, write it from memory |
| **拼音** (typing) | The reading, e.g. `fàn guǎn` | Type the characters with your IME |

拼音 mode needs a Chinese input method: switch your keyboard to pinyin input. Matching is
prefix-based, so an IME committing characters at its own pace — or backspacing to fix a
mistake — is handled without false errors.

In 听写 and 拼音 the word stays masked as `＿＿` and reveals one character at a time as you get
each one right, so you still get feedback without being shown the answer. `再听一次`
replays the audio.

## Features

- Enter any number of words — commas (`，` or `,`), spaces, and newlines all work
- **🎲 Random 5** draws five words from a built-in bank of 186 common words
  (people, places, time, food, feelings, actions, travel, weather, school, tech,
  colours, position) — every character verified against the stroke-data index
- Automatic progression: character → character → word → results
- Pinyin shown per character and per word, and again on the results screen —
  toggleable, and context-aware (重要 reads *zhòng yào*, not *chóng yào*)
- Per-character mistake tracking, with a "redo the ones I missed" pass
- 米字格 guide lines behind every box
- Adjustable strictness, hint-after-N-misses, and auto-advance so a hard stroke
  never dead-ends you
- Synthesised sound effects (Web Audio — no audio files): per-stroke blip, mistake
  buzz, per-character chime, per-word arpeggio, closing fanfare. Mutable.
- Responsive and touch-friendly: verified at 375×812 with no horizontal page scroll on
  any screen. Writing squares lay out on a grid whose column count is chosen so a wrapped
  word splits evenly (four characters read 2×2, not 3+1), and `touch-action: none` keeps
  the page from scrolling under your finger mid-stroke.

## Spaced repetition

Missed characters come back on their own. Every character you attempt is graded into a
Leitner box; a miss (or a skip) drops it to box 1 and makes it **due immediately**, while
a clean pass promotes it and pushes it further out:

| Box | 1 | 2 | 3 | 4 | 5 | 6 |
|---|---|---|---|---|---|---|
| Next due | now | 20 min | 1 day | 3 days | 7 days | 21 days |

Due characters surface two ways: a banner on the setup screen with a **Review them**
button, and — with *Mix in due reviews* on — up to three due words folded to the front of
whatever session you start. The mix is always announced on screen rather than done
silently. Reviews use the word the character was first met in, so you practise 学习
rather than a bare 学.

## Progress

**📊 Progress** (on the setup and results screens) keeps a history of every finished
session: headline totals, an accuracy trend for the last 20 sessions, a day streak, and
a "characters to work on" list ranked by how often each one trips you up.

Accuracy is `correct ÷ (correct + mistakes)`, where "correct" counts strokes in 描红 and
听写 but characters in 拼音 — so compare rows within a mode, not across them. The mode is
shown on every row.

History lives in `localStorage`, so it is **per-browser and private to this device**.
There is no server, which also means there is no cross-user leaderboard. Clearing your
browser data — or opening the page in private mode — loses it.

## Theme

The button in the top right cycles **auto → light → dark**, remembered between visits.
**Light is the default**, including on a dark-mode OS; pick *Auto* to follow the system.

Light mode is styled as 宣纸 rice paper: a warm pulp ground with fibre tooth, an uneven
wash, sumi-ink text and a 朱砂 vermilion accent for the active writing square. The
texture is generated entirely in CSS from an inline SVG `feTurbulence` filter composited
with `background-blend-mode` — no image files and no extra network requests. Dark mode
drops the paper entirely for a plain ink-stone palette.

## Running it

There is no build step and no dependencies to install. Open `index.html` in a browser,
or serve the folder:

```bash
python3 -m http.server 8000
```

Character stroke data is fetched on demand from the jsDelivr CDN, so the first load of
each character needs a network connection.

## 听写 voice support

Dictation uses the browser's built-in `speechSynthesis` — no API key, no server. The
available voices come from the **operating system**, not from this app:

| Platform | Mandarin voices |
|---|---|
| macOS / iOS | Excellent (Tingting, Li-Mu, Meijia, …) |
| Android Chrome | Usually available |
| Windows | Only with the Chinese language pack installed |
| Desktop Linux | Often none |

If no Mandarin voice is found, the app says so on the setup screen rather than failing
silently. Guaranteed audio for every visitor would require a server-side TTS API.

## Built on

- [hanzi-writer](https://github.com/chanind/hanzi-writer) (MIT) — stroke animation and
  stroke-order grading
- [Make Me a Hanzi](https://github.com/skishore/makemeahanzi) — stroke-order vector and
  median data for ~9,000 characters
- [pinyin-pro](https://github.com/zh-lx/pinyin-pro) (MIT) — word-level pinyin with
  多音字 disambiguation

## Deploying

Any static host works. For Vercel:

```bash
vercel --prod
```

## License

MIT
