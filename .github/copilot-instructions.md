# Copilot Instructions — Sasha (Cardboard Robot Puppy)

## Project Overview

Sasha is a cardboard robot puppy built as a parent-child learning project. The hardware is a Raspberry Pi Zero 2 W controlling DC motors (locomotion), servos (tail/head), NeoPixel LED eyes, sensors (ultrasonic, touch, USB mic), a Pi Camera v2, and a speaker. AI processing (vision, speech-to-text) runs on a separate MacBook via Ollama, with OpenAI API as a pluggable alternative.

See `ROADMAP.md` for the full architecture, hardware list, pin assignments, and build plan.

## Architecture

- `sasha/` — Main Python code that runs on the Raspberry Pi
- `server/` — FastAPI AI server that runs on a MacBook (Ollama + Whisper)
- `notebooks/` — Jupyter notebooks for teaching/learning
- `templates/` — Printable cardboard body templates

## Key Conventions

- **AI provider abstraction:** `sasha/brain/ai_client.py` defines the interface. Ollama and OpenAI are swappable backends. Never call a specific provider directly from behavior code.
- **Behavior engine:** Behaviors in `sasha/behavior/` are prioritized (obstacle avoidance > voice commands > gestures > mood-driven idle). New behaviors must register with the engine in `engine.py`.
- **Mood state machine:** States are `happy`, `curious`, `scared`, `sleepy`. Mood affects eye color, speed, and sound selection. Transitions are event-driven.
- **Hardware pin assignments:** All GPIO pins are defined in `sasha/config.py`. Never hardcode pin numbers elsewhere.
- **Target audience:** Code should be readable by a beginner. Prefer clarity over cleverness. Comments should explain *why*, not *what*.

## Tech Stack

- **Pi code:** Python 3, `gpiozero` or `pigpio` for GPIO, `picamera2` for camera, `neopixel` / `rpi_ws281x` for LEDs
- **AI server:** FastAPI, `ollama` Python client, `openai-whisper`
- **Notebooks:** Jupyter, targeting Python 3
