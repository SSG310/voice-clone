# AI Voice Clone

A terminal-based voice assistant that listens through your microphone, transcribes
your speech with AssemblyAI, generates a response through AssemblyAI's LLM
Gateway, and speaks the result using an AssemblyAI cloned voice.

## Features

- Records microphone audio until you press Enter
- Transcribes speech with AssemblyAI
- Keeps a short in-memory conversation history
- Generates concise, conversational programming and AI answers
- Streams responses to your speakers in a cloned voice
- Says goodbye when you include "goodbye" in your request

## Requirements

- Python 3.14 or newer
- `uv` package manager
- A working microphone and speakers
- An AssemblyAI API key
- An AssemblyAI cloned voice ID
- PortAudio, required by `sounddevice` on Linux and some macOS setups

## Setup

1. Clone the repository and enter the project directory:

   ```bash
   git clone https://github.com/SSG310/voice-clone.git
   cd voice-clone
   ```

2. Install dependencies:

   ```bash
   uv sync
   ```

3. Create a `.env` file in the project root:

   ```dotenv
   ASSEMBLYAI_API_KEY=your_assemblyai_api_key
   ASSEMBLYAI_VOICE_ID=your_cloned_voice_id
   ```

   Keep this file private. It is excluded by `.gitignore`.

On Debian or Ubuntu, install the system audio dependency when needed:

```bash
sudo apt install libportaudio2
```

On macOS with Homebrew:

```bash
brew install portaudio
```

## Run

Start the assistant with:

```bash
uv run main.py
```

The terminal will guide you through each turn:

1. Press Enter to begin recording.
2. Speak into the microphone.
3. Press Enter again to stop recording.
4. The assistant transcribes and prints your message.
5. The generated answer is printed and played through your speakers.

Say **"goodbye"** to end the session. The assistant's default model is
`qwen3.5-4b-32k-fast`, which can be changed in [main.py](main.py).

## How It Works

The app uses one file, [main.py](main.py), for the complete interaction:

- `listen()` records 16 kHz, mono, 16-bit audio into `you.wav` and sends it to
  AssemblyAI for transcription.
- `think()` sends the conversation history to AssemblyAI's LLM Gateway.
- `speak()` converts the response to 24 kHz streaming PCM audio and plays it
  through `sounddevice`.

Each successful recording replaces `you.wav` in the project directory.

## Configuration

| Variable | Purpose |
| --- | --- |
| `ASSEMBLYAI_API_KEY` | Authenticates transcription, LLM Gateway, and text-to-speech requests |
| `ASSEMBLYAI_VOICE_ID` | Identifies the cloned voice used for spoken replies |

The system prompt defines the assistant as AI Snehanshu and limits replies to
short, spoken-friendly responses. It is configured in [main.py](main.py).

## Troubleshooting

- **Missing environment variables:** confirm that `.env` exists in the project
  root and contains both variables.
- **No microphone found:** check the system input device and ensure the
  terminal application has microphone permission.
- **No audio output:** verify that the correct speaker device is selected on
  the operating system.
- **PortAudio or `sounddevice` import errors:** install PortAudio using the
  platform-specific commands above, then run `uv sync` again.
- **No transcription result:** speak clearly, increase microphone volume, and
  check that your AssemblyAI account has API access.

## Project Files

```text
voice-clone/
├── main.py       # Voice assistant implementation
├── pyproject.toml
├── uv.lock
├── README.md
└── .gitignore
```