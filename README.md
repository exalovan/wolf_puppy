# 🐕 Sasha — Cardboard Robot Puppy

A parent-child robotics project to build an AI-powered cardboard robot puppy. Sasha can see, hear, move, and express emotions — all built from cardboard, a Raspberry Pi, and a handful of motors and sensors.

## What is this?

Sasha is a hands-on learning project designed for a parent (with coding experience) and a 9-year-old to build together over 5 weeks. The puppy:

- **Moves** — wheels hidden inside legs, differential steering
- **Sees** — Pi Camera + AI vision (person detection, gesture recognition)
- **Hears** — USB microphone + speech-to-text for voice commands
- **Feels** — ultrasonic distance sensor (whiskers) + touch button (petting)
- **Expresses** — NeoPixel LED eyes (mood colors), servo-driven tail wag and head movement, speaker (barks, whimpers, speech)
- **Thinks** — AI processing via Ollama on a local MacBook (with OpenAI API as pluggable alternative)

## Learning Objectives

- Understand hardware components and how they connect
- Code in Python: if→then logic vs. AI-driven learned behavior
- Integrate sensors, motors, and AI into a working system
- Experience the full build cycle: design → prototype → assemble → code → test

## Architecture

```
┌─────────────────────────────────────┐
│         Sasha (Cardboard Body)      │
│                                     │
│  Pi Camera ──▶ Pi Zero 2 W ◀── USB Mic
│                    │                │
│          ┌────────┼────────┐       │
│          ▼        ▼        ▼       │
│     NeoPixel   DRV8833   Servos    │
│     Eyes ×2    Motor Drv  (×3)     │
│                 │    │    │  │  │  │
│              Motor Motor Tail Head │
│              (L)   (R)   Wag Tilt  │
│                                     │
│  + HC-SR04 (distance/nose)         │
│  + Touch button (pet sensor/head)  │
│  + Speaker + amp (voice/mouth)     │
└─────────────────────────────────────┘
         │ WiFi
         ▼
┌─────────────────────────────────────┐
│  MacBook (AI Server)                │
│  Ollama (vision) + Whisper (voice)  │
└─────────────────────────────────────┘
```

## Hardware Summary

| Category | Key Components |
|----------|---------------|
| Brain | Raspberry Pi Zero 2 W |
| Vision | Pi Camera Module 2 |
| Movement | 2× DC gear motors (rear legs) + 3× SG90 servos (tail, head tilt, head turn) |
| Sensors | HC-SR04 ultrasonic, touch button, USB microphone |
| Output | 2× NeoPixel 8-LED rings (eyes), speaker + PAM8403 amp |
| Power | USB power bank (Pi) + 4×AA / 18650 pack (motors, servos, LEDs) |
| Body | Template-based cardboard construction, reinforced with popsicle sticks |
| AI Server | MacBook Pro M1 running Ollama (Llama 3.2 Vision) + Whisper |

**Estimated cost:** ~$195–250 (kit-based) or ~$210–270 (all individual parts)

## Project Structure

```
wolf_puppy/
├── README.md               ← You are here
├── ROADMAP.md              ← Detailed execution plan, shopping list, wiring, week-by-week schedule
├── notebooks/              ← Jupyter notebooks for learning (step-by-step lessons)
├── sasha/                  ← Main puppy code (runs on Pi Zero 2 W)
│   ├── main.py             ← Entry point
│   ├── config.py           ← Pin assignments, settings
│   ├── hardware/           ← Motor, servo, sensor, LED, speaker drivers
│   ├── brain/              ← AI client (Ollama/OpenAI abstraction), vision, voice
│   └── behavior/           ← Behavior engine, mood state machine, rules
├── server/                 ← FastAPI AI server (runs on MacBook)
└── templates/              ← Printable cardboard body templates (PDF)
```

## Getting Started

1. **Read [ROADMAP.md](ROADMAP.md)** — full hardware shopping list (with Amazon links), wiring diagrams, GPIO pin assignments, and the 5-week build schedule
2. **Order parts** (Week 0) — see the shopping list in ROADMAP.md
3. **Set up the Pi** — flash Raspberry Pi OS Lite (64-bit), enable WiFi/SSH/camera
4. **Set up AI server** — install Ollama on MacBook, pull `llama3.2-vision:11b`
5. **Follow the weekly plan** — each week has concrete sessions and checkpoints

## Tech Stack

- **Python 3** — all code (Pi + server + notebooks)
- **Raspberry Pi OS Lite (64-bit)** — on the Pi Zero 2 W
- **gpiozero / pigpio** — GPIO and motor/servo control
- **picamera2** — camera capture
- **rpi_ws281x** — NeoPixel LED control
- **FastAPI** — AI server on MacBook
- **Ollama** — local LLM for vision (Llama 3.2 Vision 11B)
- **OpenAI Whisper** — speech-to-text (local)
- **OpenAI API** — pluggable cloud alternative for vision/voice

## License

See [LICENSE](LICENSE) for details.