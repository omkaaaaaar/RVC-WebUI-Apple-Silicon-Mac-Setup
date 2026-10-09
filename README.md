# RVC WebUI for Apple Silicon macOS

A macOS-focused customization of an existing Retrieval-based Voice Conversion (RVC) WebUI, configured for local development and experimentation on Apple Silicon Macs.

This project documents my work adapting the existing RVC WebUI to my local environment, troubleshooting dependency and compatibility issues, configuring inference assets, and building a reproducible local voice-conversion workflow.

## Overview

Retrieval-based Voice Conversion (RVC) is an open-source approach to AI-based voice conversion. This project builds on an existing RVC WebUI rather than implementing the underlying voice-conversion architecture from scratch.

### Project Goals

- Run the RVC WebUI locally on Apple Silicon macOS.
- Document a reproducible Python environment setup.
- Resolve dependency installation and compatibility issues encountered on macOS.
- Configure the model assets required for inference.
- Experiment with local voice-conversion workflows.

## My Contributions

My work focuses on adapting and documenting the existing project for my local development environment:

- Set up a dedicated Python 3.10 virtual environment on Apple Silicon macOS.
- Troubleshot native dependency builds and Python package compatibility.
- Installed additional dependencies required by the WebUI and inference pipeline.
- Configured required HuBERT and pitch-extraction assets.
- Captured installed Python package versions in a platform-specific lock file.
- Documented environment-specific fixes and setup requirements.

This is a customization of an existing open-source project, not an implementation of RVC from scratch.

## Tech Stack

- Python 3.10
- PyTorch
- Fairseq
- librosa
- Gradio
- NumPy and SciPy
- FAISS
- ONNX Runtime
- FFmpeg
- macOS on Apple Silicon

## Development Environment

| Component           | Configuration         |
| ------------------- | --------------------- |
| Operating system    | macOS                 |
| Architecture        | Apple Silicon (arm64) |
| Python              | 3.10.22               |
| pip                 | 24.0                  |
| Virtual environment | Python venv           |
| Audio processing    | FFmpeg                |
| Interface           | Local Gradio WebUI    |

The environment was configured on an Apple Silicon Mac. Other macOS versions and hardware configurations may require additional changes.

## Key Dependency Versions

The following versions were recorded from the development environment:

| Package     | Version |
| ----------- | ------- |
| torch       | 2.14.1  |
| torchaudio  | 2.11.0  |
| fairseq     | 0.12.2  |
| numpy       | 2.2.6   |
| scipy       | 1.13.1  |
| librosa     | 0.11.0  |
| gradio      | 6.29.1  |
| faiss-cpu   | 1.15.1  |
| av          | 17.1.0  |
| onnxruntime | 1.23.2  |
| pybase16384 | 0.3.9   |

The complete set of installed Python package versions is recorded in `requirements-macos-arm64.lock.txt`.

## Prerequisites

- Apple Silicon Mac
- Homebrew
- Python 3.10
- FFmpeg
- Git
- Xcode Command Line Tools

Install the main prerequisites:

```bash
brew install python@3.10 ffmpeg
xcode-select --install
```

Verify the installations:

```bash
/opt/homebrew/bin/python3.10 --version
ffmpeg -version
```

## Installation

### 1. Clone the repository

Replace the placeholders with your GitHub repository URL and directory name.

```bash
git clone <YOUR-GITHUB-REPOSITORY-URL>
cd <YOUR-REPOSITORY-DIRECTORY>
```

### 2. Create and activate a virtual environment

```bash
/opt/homebrew/bin/python3.10 -m venv .venv
source .venv/bin/activate
```

Verify the active interpreter:

```bash
python --version
which python
```

### 3. Install the compatible pip version

This environment was configured with pip 24.0 to work around installation issues involving older dependencies.

```bash
python -m pip install "pip<24.1"
```

### 4. Configure the macOS SDK

For native packages that require compilation, configure the SDK path:

```bash
export SDKROOT="$(xcrun --sdk macosx --show-sdk-path)"
```

### 5. Install the locked Python dependencies

The lock file records the installed package versions captured from the working development environment.

```bash
python -m pip install -r requirements-macos-arm64.lock.txt
```

Check for dependency conflicts:

```bash
python -m pip check
```

**Important:** The lock file records Python packages, not every system-level dependency or manual code modification. Installation on another machine may still require additional configuration.

### 6. Configure inference assets

RVC inference requires compatible model assets. Depending on the repository version and inference configuration, these may include:

- HuBERT feature-extraction weights
- Pitch-extraction model files
- An RVC voice-conversion checkpoint
- An optional retrieval index

Follow the original project's instructions for obtaining the required assets. A successful source-code clone does not necessarily include every model file.

Custom RVC checkpoints are typically placed in:

```text
assets/weights/
```

The exact paths may differ by repository version. Use the configuration and error messages from your checked-out source to determine which files are required.

Large checkpoints, private credentials, and generated audio should not be committed to Git.

### 7. Launch the WebUI

Activate the virtual environment and run:

```bash
python web.py --port 7860
```

Open the interface in your browser:

[http://localhost:7860](http://localhost:7860)

Keep the terminal running while using the WebUI.

## macOS Compatibility Notes

### Python compatibility

Python 3.10 was selected because older RVC dependencies may not work consistently with newer Python versions.

### Native dependency builds

Some packages, including PyWorld, require native compilation tools and a correctly configured macOS SDK.

### Fairseq and PyTorch

Older Fairseq checkpoint-loading code can require compatibility adjustments when used with newer PyTorch releases. A local source-code patch was applied during development to address a checkpoint-loading issue.

That patch is separate from the Python dependency lock file. Its applicability depends on the exact Fairseq source version and the checkpoint being loaded.

### Model compatibility

Model checkpoints are not interchangeable across all implementations. The model architecture expected by the inference code must match the checkpoint format.

### Dependency warnings

The environment may display deprecation warnings from older packages. A warning is not necessarily an installation failure, but compatibility should be validated before upgrading dependencies.

## Dependency Management

The repository maintains a platform-specific package snapshot:

```text
requirements-macos-arm64.lock.txt
```

After intentionally changing dependencies and verifying that the application still works, regenerate the snapshot:

```bash
source .venv/bin/activate
python -m pip freeze > requirements-macos-arm64.lock.txt
python -m pip check
```

Review changes to the lock file before committing them.

The lock file does not capture FFmpeg, the macOS SDK, downloaded model assets, environment variables, or modifications to installed source code.

## Project Status

**Status: Work in progress**

The local WebUI has been launched successfully, and a custom voice-conversion checkpoint has been detected by the interface.

End-to-end inference and output quality are still being validated. Successful conversion and compatibility across other Mac configurations have not yet been established.

## Repository Structure

Important locations in the upstream project include:

```text
.
├── assets/
│   ├── hubert/
│   ├── rmvpe/
│   └── weights/
├── requirements/
│   └── gui.txt
├── requirements-macos-arm64.lock.txt
├── web.py
└── README.md
```

The exact structure may differ between repository versions.

## Credits and Attribution

This project is based on the [RVC WebUI macOS project](https://github.com/drycen/rvc-webui-macos) and the broader [RVC project](https://github.com/RVC-Project/Retrieval-based-Voice-Conversion-WebUI).

The original authors retain credit for their underlying implementations. My contribution focuses on adapting the environment for local Apple Silicon development, troubleshooting dependencies, configuring inference assets, and documenting the setup process.

Please consult the original repositories' licenses and notices before redistributing code or assets.

## Responsible Use

Use voice models and generated audio only when you have the necessary permissions and rights. Respect the licenses and terms applicable to source code, model checkpoints, training data, and generated content.

This repository is intended for technical experimentation and educational purposes.
# RVC-WebUI-Apple-Silicon-Mac-Setup
