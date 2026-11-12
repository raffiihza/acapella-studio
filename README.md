# Acapella Studio

Record a multi-part a cappella video in the browser, one part at a time. Each new take plays back
against the parts you already have, and the result exports as a single video with every part in a grid.

Everything runs on your device. Recordings are kept in the browser's own storage (IndexedDB) and are
never uploaded.

## Use it

Open `index.html` over `https://` or on `localhost`. Browsers only allow the camera and microphone on
a secure page, so opening the file straight from disk will not record.

```sh
python3 -m http.server 8000   # then open http://localhost:8000
```

Any static host works (GitHub Pages, Netlify, and so on). There is nothing to build.

1. Put on wired headphones and press **Record**. Record the bass or a guide first.
2. Record the next part. You hear the earlier parts while you sing.
3. Use **Delay** on a part to nudge it earlier or later. The waveform lanes show how the parts line up.
4. Press **Export**.

Other things it does:

- Video or audio-only takes, and re-recording a single part.
- **Add file** brings in a backing track or an existing video.
- Volume, pan, mute and solo per part. A part can be left out of the mix without deleting it.
- A time bar to play from any point.
- Count-in, metronome, and latency calibration for tight sync.
- 9:16, 16:9, 1:1 and 4:5 frames.
- With no video parts in the mix, Export saves an audio file.

Keys: `R` records or stops, `Space` plays or pauses.

## Browser support

Export renders frame by frame with WebCodecs where the browser has it, and falls back to a real-time
screen capture where it does not. The output is MP4 (H.264) when the browser can encode it, otherwise
WebM.

Recordings live in browser storage, so clearing site data deletes them. Download the parts you care
about, or export, before you do.

## Code

The whole app is `index.html`: styles, markup, the app script, and a bundled subset of
[Mediabunny](https://github.com/Vanilagy/mediabunny) (MPL-2.0) for reading and writing media files.
