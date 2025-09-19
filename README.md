# EdgeVoice - OpenAI-Compatible Edge-TTS API


Free OpenAI-compatible text-to-speech API using Microsoft Edge's TTS service.

## Features

- **OpenAI-Compatible**: Same endpoint `/v1/audio/speech` and parameters
- **Free**: Uses Microsoft Edge TTS (no API costs)
- **Multiple Formats**: mp3, opus, aac, flac, wav, pcm
- **Speed Control**: 0.25x to 4.0x playback speed
- **Streaming**: Server-Sent Events support
- **Voice Options**: OpenAI voices (alloy, echo, fable, onyx, nova, shimmer) or [any edge-tts voice](https://www.tts.best/)

## ⚡️ Quick Start

```bash
git clone https://github.com/OOMrConrado/EdgeVoice.git
cd EdgeVoice
docker compose up -d
```

Service runs at `http://localhost:5050`

## Usage

Basic example:

```bash
curl -X POST http://localhost:5050/v1/audio/speech \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer any-key-works" \
  -d '{
    "input": "Hello world!",
    "voice": "alloy"
  }' \
  --output speech.mp3
```

## Configuration

**Optional**: Copy and modify environment variables:

```bash
cp .env.example .env
```

**Optional**: Enable FFmpeg for additional formats:

```bash
INSTALL_FFMPEG_ARG=true docker compose up --build -d
```

## Alternative Installation

<details>
<summary>Running with Python</summary>

```bash
git clone https://github.com/OOMrConrado/EdgeVoice.git
cd EdgeVoice

# Set up virtual environment
python3 -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt

# Copy config
cp .env.example .env

# Run server
python app/server.py
```

</details>

## API Reference

**Endpoint**: `POST /v1/audio/speech`

**Required**:
- `input` - Text to convert (max 4096 chars)

**Optional**:
- `voice` - alloy, echo, fable, onyx, nova, shimmer (default: alloy)
- `response_format` - mp3, opus, aac, flac, wav, pcm (default: mp3)
- `speed` - 0.25 to 4.0 (default: 1.0)
- `stream_format` - "audio" or "sse" (default: audio)

**Other Endpoints**:
- `GET /v1/models` - List available models
- `GET /v1/voices` - List available voices

<details>
<summary>Advanced Examples</summary>

**Server-Sent Events streaming**:
```bash
curl -X POST http://localhost:5050/v1/audio/speech \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer any-key" \
  -d '{
    "input": "Streaming example",
    "voice": "alloy",
    "stream_format": "sse"
  }'
```

**Direct playback with ffplay**:
```bash
curl -X POST http://localhost:5050/v1/audio/speech \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer any-key" \
  -d '{
    "input": "Direct playback example",
    "voice": "shimmer"
  }' | ffplay -autoexit -nodisp -i -
```

**International voices**:
```bash
curl -X POST http://localhost:5050/v1/audio/speech \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer any-key" \
  -d '{
    "input": "じゃあ、行く。電車の時間、調べておくよ。",
    "voice": "ja-JP-KeitaNeural"
  }' \
  --output japanese.mp3
```

</details>

## Tips

- No real API key required - use any string
- For network access, replace `localhost` with your IP (e.g., `192.168.0.1`)
- Docker required for containerized setup

## License

GPL-3.0 - Personal use only

---

**[Voice Samples](https://www.tts.best/) - Test all available Edge TTS voices**
