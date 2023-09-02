# Chess Simp transcripts

This repository hosts transcripts for all Chess Simp videos as of August 29, 2023.

I made this so that I could find a video where he says "Maybe Not" in an expressive voice. Unfortunately it turned out that he always says it in the same way.

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

mkdir srt
mv *.srt srt/
```

### Converting to plaintext

This is if you want to remove the timestamps.

```bash
mkdir plaintext
for srt in srt/*.srt; do
  txt="plaintext/$(basename "$srt" .srt).txt" 
  grep -v '^[0-9]' "$srt" | sed '/^$/d' | sed 's/<[^>]*>//g' | tr '\n' ' ' > "$txt"
done
```

