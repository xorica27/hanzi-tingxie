# 汉字 Writing Quiz

A single-file web app for practising Chinese character handwriting. Type the words you
want to drill, then write each character stroke by stroke and get graded on stroke order.

**Live:** https://hanzi-tingxie.vercel.app

## Two modes

| Mode | What you see | What you do |
|---|---|---|
| **描红** (trace) | The character outline | Trace over it |
| **听写** (dictation) | An empty box | Listen to the word, write it from memory |

In 听写 the word stays masked as `＿＿` and reveals one character at a time as you get
each one right, so you still get feedback without being shown the answer. `再听一次`
replays the audio.

## Features

- Enter any number of words — commas (`，` or `,`), spaces, and newlines all work
- Automatic progression: character → character → word → results
- Per-character mistake tracking, with a "redo the ones I missed" pass
- 米字格 guide lines behind every box
- Adjustable strictness, hint-after-N-misses, and auto-advance so a hard stroke
  never dead-ends you
- Synthesised sound effects (Web Audio — no audio files): per-stroke blip, mistake
  buzz, per-character chime, per-word arpeggio, closing fanfare. Mutable.
- Dark mode, responsive, touch-friendly

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

## Deploying

Any static host works. For Vercel:

```bash
vercel --prod
```

## License

MIT
