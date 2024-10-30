# VoiceForge Studio

A web application for zero-shot voice cloning and text-to-speech. Upload a short voice sample, enter any text, and generate speech in that voice using the F5-TTS or E2-TTS models.

## Features

- **Voice cloning** from a short reference clip (WAV or MP3)
- **Automatic transcription** of the reference audio with faster-whisper
- **Two models:** F5-TTS (Diffusion Transformer) and E2-TTS (flat UNet Transformer)
- **Long text support:** text is split into batches automatically, and the generated clips are joined together
- **Automatic audio cleanup:** silence is trimmed, and clips longer than 15 seconds are cut down
- **Command-line inference** for scripting and batch use
- **Playback and download** of the generated audio in the browser

## Tech Stack

| Layer | Technology |
|---|---|
| Backend | Python, Flask |
| Frontend | HTML, JavaScript, Tailwind CSS |
| TTS models | F5-TTS, E2-TTS (flow matching) |
| Vocoder | Vocos |
| Transcription | faster-whisper |
| Audio processing | librosa, soundfile, pydub, torchaudio |

## Installation

**1. Clone the repository**

```bash
git clone https://github.com/zaynnmotiwala/voiceforge-studio.git
cd voiceforge-studio
```

**2. Create an environment (Python 3.10)**

```bash
conda create -n voiceforge python=3.10
conda activate voiceforge
```

**3. Install PyTorch.** Pick the build that matches your CUDA version from [pytorch.org](https://pytorch.org/get-started/locally/). For example:

```bash
pip install torch torchaudio --index-url https://download.pytorch.org/whl/cu121
```

**4. Install the dependencies**

```bash
pip install -r requirements.txt
```

The model checkpoints download automatically the first time you run the app.

## Usage

### Web app

```bash
python app.py
```

Open **http://localhost:5000**, then:

1. Upload a reference audio clip (ideally 5–15 seconds of clear speech).
2. Check the transcription the app generates, and edit it if needed.
3. Enter the text you want to generate.
4. Choose **F5-TTS** or **E2-TTS**.
5. Click **Generate Audio**, then play or download the result.

### Command line

Edit the defaults in `inference-cli.toml`, or pass options directly:

```bash
python inference-cli.py \
  --model F5-TTS \
  --ref_audio tests/ref_audio/test_en_1_ref_short.wav \
  --ref_text "Some call me nature, others call me mother nature." \
  --gen_text "Hello! This voice was generated with VoiceForge Studio." \
  --output_dir tests
```

| Option | Description |
|---|---|
| `-m, --model` | `F5-TTS` or `E2-TTS` |
| `-r, --ref_audio` | Reference audio file (under 15 s) |
| `-s, --ref_text` | Transcript of the reference audio (leave empty `""` to transcribe automatically) |
| `-t, --gen_text` | Text to generate |
| `-f, --gen_file` | Text file to generate from (overrides `--gen_text`) |
| `-o, --output_dir` | Output folder |
| `--remove_silence` | Remove long silences from the output |

## Project Structure

```text
├── app.py               # Flask web app (upload, transcribe, generate)
├── templates/           # Web interface
├── inference-cli.py     # Command-line inference
├── inference-cli.toml   # CLI default settings
├── speech_edit.py       # Speech editing script
├── train.py             # Model training script
├── model/               # CFM, DiT, UNetT, MMDiT model code
├── scripts/             # Dataset preparation and evaluation
├── data/                # Vocab and evaluation lists
└── tests/ref_audio/     # Sample reference audio
```

## Notes

- A GPU is recommended. The app also runs on CPU, but generation is much slower.
- The best results come from clean reference audio with a single speaker and no background noise.
- Only clone voices you have permission to use.


