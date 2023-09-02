# Chess Simp transcriptions

This repository hosts transcriptions for all Chess Simp videos as of August 29, 2023.

I made this so that I could find a video where he says "Maybe Not" in an expressive voice.

## How this was made

### Whisper

```bash
git clone https://github.com/ggerganov/whisper.cpp
cd whisper.cpp
./models/download-ggml-model.sh large
make
export WHISPER="$(pwd)"
```

### Transcribing

```bash
yt-dlp --extract-audio --audio-format wav "https://www.youtube.com/@ChessSimp"

for file in *.wav; do
  sox "$file" -r 16000 "${file%.wav}_16k.wav"
  rm "$file"
done

"$WHISPER/main" -m "$WHISPER/models/ggml-large.bin" -osrt -l en *.wav
```

