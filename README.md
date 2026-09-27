# Speech & Vocal Noise Reduction with MossFormer2

**English** | [Tiếng Việt](README_VI.md)

[![Python](https://img.shields.io/badge/Python-3.8%2B-blue.svg)](https://www.python.org/)
[![Model](https://img.shields.io/badge/Model-MossFormer2__SE__48K-brightgreen.svg)](https://github.com/modelscope/ClearerVoice-Studio)
[![Audio](https://img.shields.io/badge/Audio-48kHz%20Full--Band-orange.svg)](#)
[![License](https://img.shields.io/badge/License-Apache%202.0-lightgrey.svg)](#)

A high-fidelity speech enhancement and background noise suppression pipeline powered by **[ClearVoice](https://github.com/modelscope/ClearerVoice-Studio)** and the state-of-the-art **MossFormer2_SE_48K** deep learning model.

This project delivers studio-quality, monophonic speech denoising at a **48 kHz** sampling rate, making it ideal for podcast cleanups, vocal recordings, audio post-production, and AI voice pre-processing.

---

## Key Features

- **48 kHz High-Definition Audio**: Operates across the full acoustic frequency range (0–24 kHz) to preserve natural vocal timbre and avoid muffled outputs.
- **State-of-the-Art Architecture**: Uses **MossFormer2**, a recurrent, transformer-based neural network optimized for long-range sequence modeling and acoustic feature separation.
- **Zero-Config Model Management**: Pre-trained model checkpoints are automatically downloaded from ModelScope/HuggingFace on the first run.
- **Modular Pipeline**: Structured step-by-step notebook format (configuration, validation, model loading, inference, and in-notebook audio playback).
- **Google Colab & Local Ready**: Compatible with standard Python virtual environments and cloud GPU runtimes.

---

## Project Structure

```text
RENOISE/
├── remove_noise.ipynb   # Modular Jupyter notebook for noise reduction
└── README.md            # Project documentation and guide
```

---

## Quick Start

### 1. Prerequisites

- Python 3.8 – 3.10
- GPU with CUDA support (recommended for faster processing, but CPU is also supported)
- `ffmpeg` or `libsndfile` installed on your system

### 2. Installation

Install `clearvoice` and the required audio processing packages:

```bash
pip install clearvoice
pip install soundfile librosa numpy
```

---

## Usage Guide

### Method 1: Using the Jupyter Notebook (`remove_noise.ipynb`)

1. Open `remove_noise.ipynb` in **JupyterLab**, **VS Code**, or upload it to **Google Colab**.
2. Run the cells sequentially:
   - **Cell 1**: Installs dependencies.
   - **Cell 2**: Configure audio paths (`INPUT_FILE`, `OUTPUT_FILE`, and `MODEL`).
   - **Cell 3**: Validates the input audio file existence.
   - **Cell 4**: Loads the pre-trained `MossFormer2_SE_48K` model weights into memory.
   - **Cell 5**: Executes noise reduction inference.
   - **Cell 6**: Exports the clean output audio.
   - **Cell 7**: Audio preview widget to play and compare before/after tracks directly.

### Method 2: Python Script Example

You can also run the pipeline in a standalone Python script:

```python
import os
import time
from clearvoice import ClearVoice

# Configuration
INPUT_FILE = "path/to/your/recording.wav"
OUTPUT_FILE = "vocal_clean.wav"
MODEL = "MossFormer2_SE_48K"

# Verify input
if not os.path.isfile(INPUT_FILE):
    raise FileNotFoundError(f"Input file not found: {INPUT_FILE}")

# Initialize model
enhancer = ClearVoice(
    task="speech_enhancement",
    model_names=[MODEL]
)

# Process audio
print("Denoising audio...")
output_wav = enhancer(input_path=INPUT_FILE, online_write=False)

# Save result
enhancer.write(output_wav, output_path=OUTPUT_FILE)
print(f"Clean audio saved to: {os.path.abspath(OUTPUT_FILE)}")
```

---

## Configuration Options

| Parameter | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| `INPUT_FILE` | `str` | `"/content/ghiam.wav"` | Path to the source noisy audio recording (`.wav` format recommended). |
| `OUTPUT_FILE`| `str` | `"vocal_clean.wav"` | Path where the enhanced, cleaned audio will be saved. |
| `MODEL` | `str` | `"MossFormer2_SE_48K"`| Speech enhancement model checkpoint name (48 kHz monophonic). |

---

## Performance & Recommendations

- **Audio Format**: Uncompressed 16-bit or 24-bit PCM `.wav` files provide optimal processing speed and quality.
- **Sampling Rate**: The model internally resamples and outputs at 48 kHz.
- **GPU Acceleration**: A modern GPU (e.g., NVIDIA T4, RTX 3060 or higher) provides real-time or faster-than-real-time enhancement. CPU inference is supported but requires more time for longer tracks.
- **First-Time Execution**: On initial startup, the library will fetch pre-trained weights (~a few hundred megabytes). Subsequent runs will load locally cached weights instantly.

---

## References & Acknowledgments

- **ClearerVoice-Studio**: [ModelScope ClearerVoice Studio GitHub](https://github.com/modelscope/ClearerVoice-Studio)
- **MossFormer2 Paper**: *MossFormer2: Combining Recurrent and Self-Attention Mechanisms for Speech Enhancement and Separation.*

---

## License

This project is licensed under the Apache 2.0 License. Refer to the model upstream repository for model weight licensing terms.
