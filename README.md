# AI-Powered HR Call Analysis Agent

An AI-powered pipeline for analyzing recruitment and HR calls through multiple stages of speech processing, conversation analysis, and follow-up action generation.

> Originally developed as part of an AI engineering technical challenge.

## Overview

The project processes recruitment call conversations through a multi-stage analysis pipeline:

```text
Audio Recordings
      │
      ▼
Speech-to-Text
      │
      ▼
Structured Conversation
      │
      ▼
Resolution Detection
      │
      ▼
Conversation Quality Analysis
      │
      ▼
Follow-up Action Recommendation
```

The main goal is to transform raw call conversations into structured, actionable information that can help HR and recruiting teams understand the outcome and quality of each interaction.

## Pipeline

### Level 1: Audio Transcription

The first stage converts audio recordings into structured Persian transcripts.

The generated transcript representation contains:

- Session information
- Speaker identification
- Detected language
- Audio segment timestamps
- Transcribed text

The original implementation uses `SpeechRecognition` with Google's speech recognition service for Persian (`fa`).

### Level 2: Resolution Detection

The second stage determines whether a recruitment call reached a clear resolution.

The evaluation produces structured information including:

- `resolved`
- `confidence`
- `resolution_reason`
- `resolution_signal`

Supported resolution signals include:

- `قطع_تماس`
- `تماس_ناتمام`
- `درخواست_اپراتور_انسانی`
- `توافق_مرحله_اول`
- `تعیین_تماس_مجدد`

The resolution rules are explicitly defined in the prompt so that the final boolean status remains consistent with the detected resolution signal.

### Level 3: Conversation Quality Analysis

The third stage analyzes the conversation based on predefined quality criteria and produces structured JSON output.

This stage is intended to move beyond simple transcription and evaluate the quality and characteristics of the interaction.

### Level 4: Action Recommendation

The final stage uses the analysis results to determine an appropriate follow-up action.

This creates a simple decision pipeline from:

```text
Conversation
    ↓
Analysis
    ↓
Detected Outcome
    ↓
Recommended Action
```

## Tech Stack

- Python 3.11+
- SpeechRecognition
- OpenAI Python SDK
- OpenRouter API
- Qwen 2.5 7B Instruct
- JSON
- Jupyter Notebook

## Project Structure

```text
.
├── HR_Agent.ipynb
├── README.md
├── requirements.txt
└── .gitignore
```

## Reproducibility

The original technical challenge included audio samples that were used during development.

Those original audio files are **not included in this public repository**.

The public version therefore focuses on the analysis and LLM-based processing pipeline rather than distributing the original challenge dataset.

Any sample artifacts included in the public version should be treated as sanitized examples for demonstration purposes.

## Setup

### 1. Clone the repository

```bash
git clone https://github.com/Hossein-MLProjects/ai-hr-call-analysis.git
cd ai-hr-call-analysis
```

### 2. Create a virtual environment

```bash
python -m venv .venv
```

Activate it on Windows:

```powershell
.venv\Scripts\Activate.ps1
```

Or on macOS/Linux:

```bash
source .venv/bin/activate
```

### 3. Install dependencies

```bash
pip install -r requirements.txt
```

### 4. Configure the API key

The LLM analysis stage uses an OpenAI-compatible API through OpenRouter.

Set the API key as an environment variable instead of hard-coding credentials in the notebook.

Windows PowerShell:

```powershell
$env:OPENROUTER_API_KEY="your-api-key"
```

macOS/Linux:

```bash
export OPENROUTER_API_KEY="your-api-key"
```

## Model

The original implementation uses:

```text
qwen/qwen-2.5-7b-instruct
```

through an OpenAI-compatible OpenRouter endpoint.

The model is used primarily for structured conversation evaluation rather than direct speech recognition.

## Design Decisions

### Structured Intermediate Results

Each stage produces structured JSON rather than passing unstructured text between every step.

This makes the pipeline easier to inspect, debug, and extend.

### Explicit Resolution Signals

The resolution classifier does not rely only on a free-form LLM judgment.

The prompt defines an explicit set of valid resolution signals and requires the `resolved` value to remain consistent with the selected signal.

### Separation of Processing Stages

The pipeline is divided into transcription, resolution analysis, quality analysis, and action generation.

This makes each stage independently replaceable.

For example, the speech recognition component could later be replaced with a dedicated Persian ASR model without redesigning the downstream analysis stages.

## Limitations

- The original audio dataset is not included in the public repository.
- Speech recognition quality depends on the underlying transcription provider.
- LLM-based evaluation can produce variable results.
- The current implementation is notebook-oriented rather than a production service.
- The system does not currently include an automated benchmark or ground-truth evaluation suite.
- External API access is required for the LLM analysis stage.

## Future Improvements

Potential improvements include:

- Replace external speech recognition with a dedicated Persian ASR model.
- Add automated evaluation benchmarks and ground-truth labels.
- Introduce Pydantic or JSON Schema validation for model outputs.
- Add retry and timeout handling for LLM API requests.
- Add structured logging.
- Convert the notebook into a modular Python package.
- Add unit and integration tests.
- Add an API layer for real-time or batch call processing.
- Add speaker diarization instead of relying on predefined speaker labels.
- Add model-independent evaluation so different LLMs can be compared.

## Disclaimer

This repository is a portfolio-oriented presentation of work originally completed as a technical challenge.

Original challenge audio files and potentially proprietary challenge materials are not redistributed here.
