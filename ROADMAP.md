# Sasha — Cardboard Robot Puppy 🐕

A parent-child robotics project: build a cardboard robot puppy named **Sasha** that sees, hears, moves, and has personality — powered by AI.

**Learning objectives:**
- Understand hardware components and how they connect
- Code in Python (if→then logic vs. AI-driven behavior)
- Integrate sensors, motors, and AI into a working system
- Experience the full build cycle: design → prototype → assemble → code → test

**Timeline:** 5 weeks, 2–3 hours/week (~12–15 total hours)

---

## Architecture Overview

```
┌─────────────────────────────────────────────────────────────┐
│                    Sasha (Cardboard Body)                    │
│                                                             │
│  ┌──────────────┐    ┌───────────────────────────────────┐  │
│  │ Pi Camera v2 │    │ Raspberry Pi Zero 2 W ("Brain")   │  │
│  │   (eyes)     │───▶│                                   │  │
│  └──────────────┘    │  Python main loop                 │  │
│                      │  ├─ Sensor polling                │  │
│  ┌──────────────┐    │  ├─ Behavior engine (mood/state)  │  │
│  │ USB Mic      │───▶│  ├─ Motor/servo control           │  │
│  │ (ears/hear)  │    │  └─ AI client (WiFi → MacBook)    │  │
│  └──────────────┘    └────────────┬──────────────────────┘  │
│                                   │ GPIO                     │
│  ┌────────────┐  ┌────────────┐  ┌▼───────────┐            │
│  │ NeoPixel   │  │ Ultrasonic │  │ Motor      │            │
│  │ Rings ×2   │  │ HC-SR04    │  │ Driver     │            │
│  │ (eyes/mood)│  │ (distance) │  │ DRV8833    │            │
│  └────────────┘  └────────────┘  └──┬─────┬───┘            │
│                                     │     │                  │
│  ┌────────────┐  ┌────────────┐  ┌──▼──┐ ┌▼────┐           │
│  │ Speaker +  │  │ Touch      │  │Motor│ │Motor│           │
│  │ PAM8403    │  │ Button     │  │ L   │ │ R   │           │
│  │ (voice)    │  │ (petting)  │  │(leg)│ │(leg)│           │
│  └────────────┘  └────────────┘  └─────┘ └─────┘           │
│                                                             │
│  Servos: Tail wag │ Head tilt │ Head turn                   │
│                                                             │
│  Power: USB bank (Pi) │ 4×AA pack (motors/servos/LEDs)     │
└─────────────────────────────────────────────────────────────┘
         │ WiFi (local network)
         ▼
┌─────────────────────────────────────┐
│ MacBook Pro M1 (AI Server)          │
│  ├─ Ollama (Llama 3.2 Vision 11B)  │
│  ├─ Whisper (speech-to-text)        │
│  └─ FastAPI server                  │
│     POST /vision → image analysis   │
│     POST /listen → speech-to-text   │
│                                     │
│  (Pluggable: swap to OpenAI API)    │
└─────────────────────────────────────┘
```

---

## Hardware Shopping List

Order everything in Week 0 so parts arrive by Week 1. All links are Amazon US store.

> ⚠️ **Prices are estimates** from May 2025 research. Verify current prices before purchasing. All items are Prime-eligible.

### Option A: Kit + Individual Parts (Recommended — saves money)

Start with a robot car kit that bundles motors, wheels, driver, battery holder, ultrasonic sensor, camera, and servos. Then buy only what's missing.

#### The Kit

| Part | Price | Amazon Link | Notes |
|------|-------|-------------|-------|
| **Freenove 4WD Smart Car Kit for Raspberry Pi** | ~$44–45 | [B07YD2LT9D](https://www.amazon.com/dp/B07YD2LT9D) | Includes: 4× TT motors + wheels, motor driver PCB, battery holder (18650), 1–2 servos, HC-SR04 ultrasonic, camera module, RGB LEDs, all cables/hardware, 370-page tutorial. Pi Zero 2 W listed as compatible. |

**What the kit covers from our list:** ✅ DC motors, ✅ wheels, ✅ motor driver, ✅ battery holder, ✅ ultrasonic sensor, ✅ camera, ✅ servo(s)

#### Still needed with the kit

| Part | Est. Price | Amazon Link | Notes |
|------|-----------|-------------|-------|
| Raspberry Pi Zero 2 W (pre-soldered headers) | ~$18–22 | [Search: CanaKit Pi Zero 2 W](https://www.amazon.com/s?k=Raspberry+Pi+Zero+2+W+pre-soldered+headers+CanaKit&i=electronics) | CanaKit is the most trusted US reseller |
| SanDisk 32GB Ultra microSDHC (A1) | ~$7–10 | [Search: SanDisk 32GB A1](https://www.amazon.com/s?k=SanDisk+32GB+Ultra+microSDHC+A1&i=electronics) | A1 rating critical for Pi boot performance |
| USB Micro OTG adapter | ~$6–9 | [Search: UGREEN micro USB OTG](https://www.amazon.com/s?k=UGREEN+micro+USB+OTG+adapter+female+to+micro+USB+male&i=electronics) | UGREEN 2-pack, for USB mic connection |
| USB mini microphone | ~$8–15 | [Search: USB mini mic Raspberry Pi](https://www.amazon.com/s?k=USB+mini+microphone+Raspberry+Pi+plug+and+play&i=electronics) | Plugable USB Audio Adapter or similar. Must be USB class-compliant (no drivers) |
| SG90 micro servo (4-pack) | ~$9–11 | [Search: SG90 servo](https://www.amazon.com/sg90-9g-micro-servo/s?k=sg90+9g+micro+servo) | Miuzei or Hosyond brand. Kit may include 1–2, buy extra to reach 3 total |
| Ball caster wheels (pack of 4) | ~$6–10 | [Search: ball caster robot](https://www.amazon.com/s?k=universal+ball+caster+wheel+robot+arduino+small) | For front leg "paws" |
| NeoPixel ring 8-LED WS2812 (2-pack) | ~$9–11 | [Search: WS2812 8 LED ring](https://www.amazon.com/s?k=WS2812B+8+LED+ring) | BTF-LIGHTING brand. Animated mood eyes |
| PAM8403 mini amplifier (2-pack) | ~$6–7 | [B00LODGV64](https://www.amazon.com/dp/B00LODGV64) | HiLetgo brand, includes volume knob |
| Mini speaker 8Ω 0.5W (6-pack) | ~$10 | [B0D8Q4XZ14](https://www.amazon.com/dp/B0D8Q4XZ14) | Treedix brand with JST connector |
| Tactile push buttons (100-pack) | ~$8 | [B079JM1SN4](https://www.amazon.com/dp/B079JM1SN4) | HiLetgo 6×6mm, breadboard-compatible |
| 10kΩ resistors (100-pack) | ~$6 | [B01DCEO4X2](https://www.amazon.com/dp/B01DCEO4X2) | Chanzon 1/4W carbon film |
| 18650 batteries (2-pack) + charger | ~$12–18 | [Search: 18650 rechargeable battery charger](https://www.amazon.com/s?k=18650+rechargeable+battery+with+charger) | Kit uses 18650s, not AA. Get protected cells |
| USB power bank (5000mAh) | ~$16–22 | [Search: Anker 521 5000mAh](https://www.amazon.com/s?k=Anker+521+power+bank+5000mAh+USB-C) | Powers Pi via USB. Anker most reliable |
| Half-size breadboard (3-pack) | ~$7–10 | [Search: ELEGOO breadboard 400](https://www.amazon.com/s?k=ELEGOO+half+size+breadboard+400+points) | For prototyping phase |
| Jumper wire kit 120pcs (M-M, M-F, F-F) | ~$6–9 | [Search: ELEGOO jumper wires](https://www.amazon.com/s?k=ELEGOO+120pcs+jumper+wire+male+female+kit) | Assorted lengths |
| Mini hot glue gun + sticks (low temp) | ~$8–14 | [Search: mini hot glue gun low temp](https://www.amazon.com/s?k=mini+hot+glue+gun+with+glue+sticks+low+temp) | Low-temp safest for kids |
| Popsicle / craft sticks (150-pack) | ~$4–7 | [Search: craft sticks 150](https://www.amazon.com/s?k=wood+craft+popsicle+sticks+150+pack) | Leg reinforcement inside cardboard |
| Zip ties assorted (100+ pack) | ~$6–9 | [Search: zip ties assorted](https://www.amazon.com/s?k=assorted+zip+ties+4+6+8+inch+100+pack+cable+management) | Wire management inside body |
| Cardboard sheets (thick corrugated) | ~$3 | Reuse shipping boxes | Or buy craft cardboard at local store |
| Paint/markers/stickers | ~$5 | She picks! | Decoration supplies |

#### **Option A estimated total: ~$195–250**

> 💡 **Note:** The kit price (~$44) replaces ~$50+ of individual motors, driver, ultrasonic, camera, and battery holder. Many items come in multi-packs (you'll have spares for mistakes or future projects).

---

### Option B: All Individual Parts (no kit)

If you prefer to buy everything separately for full control over exact components:

#### Brain & Compute

| Part | Est. Price | Amazon Link | Notes |
|------|-----------|-------------|-------|
| Raspberry Pi Zero 2 W (pre-soldered headers) | ~$18–22 | [Search: CanaKit Pi Zero 2 W](https://www.amazon.com/s?k=Raspberry+Pi+Zero+2+W+pre-soldered+headers+CanaKit&i=electronics) | CanaKit most trusted US reseller |
| SanDisk 32GB Ultra microSDHC (A1) | ~$7–10 | [Search: SanDisk 32GB A1](https://www.amazon.com/s?k=SanDisk+32GB+Ultra+microSDHC+A1&i=electronics) | A1 rating critical for Pi OS boot |
| Pi Camera Module 2 (8MP) | ~$25–30 | [Search: Pi Camera Module 2](https://www.amazon.com/s?k=Raspberry+Pi+Camera+Module+2+8MP+official&i=electronics) | Official SC0023. NOT v3 — v2 has better tutorial support |
| USB Micro OTG adapter | ~$6–9 | [Search: UGREEN micro USB OTG](https://www.amazon.com/s?k=UGREEN+micro+USB+OTG+adapter+female+to+micro+USB+male&i=electronics) | UGREEN 2-pack |
| USB mini microphone | ~$8–15 | [Search: USB mini mic Pi](https://www.amazon.com/s?k=USB+mini+microphone+Raspberry+Pi+plug+and+play&i=electronics) | Must be USB class-compliant (no drivers needed) |

#### Motors & Movement

| Part | Est. Price | Amazon Link | Notes |
|------|-----------|-------------|-------|
| TT DC gear motors 3–6V + wheels (8-pack) | ~$10 | [B0BRKJWN51](https://www.amazon.com/dp/B0BRKJWN51) | ACEIRMC brand. Verify variant includes wheels. Alt: [KEAcvise B0FPFC2WZL](https://www.amazon.com/dp/B0FPFC2WZL) |
| DRV8833 motor driver (5-pack) | ~$9 | [B07S4FVY9M](https://www.amazon.com/dp/B07S4FVY9M) | KOOBOOK, Amazon's Choice. Alt: [WWZMDiB 6-pack $8](https://www.amazon.com/dp/B0DB8CX8LK) |
| SG90 micro servo 9g (4-pack) | ~$9–11 | [Search: SG90 servo](https://www.amazon.com/sg90-9g-micro-servo/s?k=sg90+9g+micro+servo) | Miuzei or Hosyond brand |
| Ball caster wheels (pack of 4) | ~$6–10 | [Search: ball caster robot](https://www.amazon.com/s?k=universal+ball+caster+wheel+robot+arduino+small) | Front leg "paws" |

#### Sensors

| Part | Est. Price | Amazon Link | Notes |
|------|-----------|-------------|-------|
| HC-SR04 ultrasonic sensor (5-pack) | ~$9 | [B01COSN7O6](https://www.amazon.com/dp/B01COSN7O6) | ELEGOO brand. ✅ Live-verified $8.99 |
| Tactile push buttons (100-pack) | ~$8 | [B079JM1SN4](https://www.amazon.com/dp/B079JM1SN4) | HiLetgo 6×6mm DIP, breadboard-compatible |
| 10kΩ resistors (100-pack) | ~$6 | [B01DCEO4X2](https://www.amazon.com/dp/B01DCEO4X2) | Chanzon 1/4W carbon film |

#### Output (Eyes & Sound)

| Part | Est. Price | Amazon Link | Notes |
|------|-----------|-------------|-------|
| NeoPixel ring 8-LED WS2812 (2-pack) | ~$9–11 | [Search: WS2812 8 LED ring](https://www.amazon.com/s?k=WS2812B+8+LED+ring) | BTF-LIGHTING brand recommended |
| PAM8403 mini amplifier (2-pack) | ~$6–7 | [B00LODGV64](https://www.amazon.com/dp/B00LODGV64) | HiLetgo, with volume knob |
| Mini speaker 8Ω 0.5W (6-pack) | ~$10 | [B0D8Q4XZ14](https://www.amazon.com/dp/B0D8Q4XZ14) | Treedix with JST connector |

#### Power

| Part | Est. Price | Amazon Link | Notes |
|------|-----------|-------------|-------|
| USB power bank (5000mAh) | ~$16–22 | [Search: Anker 521 5000mAh](https://www.amazon.com/s?k=Anker+521+power+bank+5000mAh+USB-C) | Powers Pi via USB |
| 4×AA battery holder with switch | ~$5–8 | [Search: 4 AA battery holder switch](https://www.amazon.com/s?k=4+AA+battery+holder+with+switch+wire+leads) | Powers motors/servos/LEDs |
| AA rechargeable NiMH (4-pack) | ~$10–13 | [Search: Amazon Basics AA rechargeable](https://www.amazon.com/s?k=Amazon+Basics+AA+rechargeable+batteries+4+pack+NiMH) | 2000–2400mAh, pre-charged |

#### Construction & Wiring

| Part | Est. Price | Amazon Link | Notes |
|------|-----------|-------------|-------|
| Half-size breadboard (3-pack) | ~$7–10 | [Search: ELEGOO breadboard 400](https://www.amazon.com/s?k=ELEGOO+half+size+breadboard+400+points) | 400 tie-point, color-coded rails |
| Jumper wire kit 120pcs (M-M, M-F, F-F) | ~$6–9 | [Search: ELEGOO jumper wires 120](https://www.amazon.com/s?k=ELEGOO+120pcs+jumper+wire+male+female+kit) | Assorted lengths |
| Mini hot glue gun + sticks (low temp) | ~$8–14 | [Search: mini hot glue gun low temp](https://www.amazon.com/s?k=mini+hot+glue+gun+with+glue+sticks+low+temp) | Low-temp safest for kids |
| Popsicle / craft sticks (150-pack) | ~$4–7 | [Search: craft sticks 150](https://www.amazon.com/s?k=wood+craft+popsicle+sticks+150+pack) | Leg reinforcement |
| Zip ties assorted (100+ pack) | ~$6–9 | [Search: zip ties assorted](https://www.amazon.com/s?k=assorted+zip+ties+4+6+8+inch+100+pack+cable+management) | Wire management |
| Cardboard sheets (thick corrugated) | ~$3 | Reuse shipping boxes | Or buy craft cardboard locally |
| Paint/markers/stickers | ~$5 | She picks! | Decoration supplies |

#### **Option B estimated total: ~$210–270**

---

### Kit Comparison (if considering alternatives)

| Kit | Price | Motors | Wheels | Driver | Ultrasonic | Camera | Servos | Pi Zero 2W |
|-----|-------|--------|--------|--------|------------|--------|--------|------------|
| **Freenove 4WD** ⭐ | ~$44 | ✅ 4× | ✅ 4× | ✅ | ✅ | ✅ | ✅ 1–2 | ✅ listed |
| SunFounder PiCar-X V2 | $90 | ✅ 2× | ✅ 4× | ✅ | ✅ | ✅ | ✅ 3× | ✅ |
| DIYables 2WD Kit | ~$15–25 | ✅ 2× | ✅ 2× | ✅ L298N | ✅ | ❌ | ❌ | ✅ |

> 💡 **Recommendation:** The **Freenove 4WD kit** covers the most items at the best price. The SunFounder PiCar-X is excellent but at $90 it's nearly the cost of buying everything individually. The DIYables kit is cheap but missing camera and servos.

> ⚠️ **Freenove + Pi Zero 2 W note:** The kit lists Pi Zero 2 W as compatible, but Freenove's own store notes it "needs extra parts" — likely a different mounting bracket or adapter. Check the included 370-page PDF tutorial for Pi Zero 2 W-specific instructions before assembly.

---

## GPIO Pin Assignment

| GPIO Pin | Component | Purpose |
|----------|-----------|---------|
| GPIO 12 (PWM0) | Motor driver IN1 | Left motor forward |
| GPIO 13 (PWM1) | Motor driver IN2 | Left motor backward |
| GPIO 18 (PWM0) | Motor driver IN3 | Right motor forward |
| GPIO 19 (PWM1) | Motor driver IN4 | Right motor backward |
| GPIO 17 | Servo 1 (PWM via pigpio) | Tail wag |
| GPIO 27 | Servo 2 | Head tilt (up/down) |
| GPIO 22 | Servo 3 | Head turn (left/right) |
| GPIO 23 | HC-SR04 TRIG | Ultrasonic trigger |
| GPIO 24 | HC-SR04 ECHO (via voltage divider) | Ultrasonic echo |
| GPIO 25 | Touch button | Pet sensor (with pull-down) |
| GPIO 10 (SPI MOSI) | NeoPixel data (both rings chained) | Eye LEDs |
| Camera port | Pi Camera Module 2 | Vision |
| USB (via OTG) | USB microphone | Audio input |
| PWM audio (3.5mm) | PAM8403 → Speaker | Audio output |

---

## Puppy Behaviors

Behaviors are split into **if→then rules** (your daughter codes the logic) and **AI-powered** (the puppy asks the MacBook brain for help).

### If→Then Behaviors (coded logic)

| # | Behavior | Trigger | Reaction |
|---|----------|---------|----------|
| 1 | **Wag when petted** | Touch button pressed | Tail servo wags, eyes turn green, play happy bark |
| 2 | **Avoid obstacles** | Ultrasonic < 15cm | Stop, back up, turn randomly, eyes turn yellow |
| 3 | **React to loud sounds** | Mic amplitude > threshold | Head tilts, eyes flash blue, curious bark |
| 4 | **Mood system** | Internal state machine | Affects eye color, movement speed, sound selection |

### AI-Powered Behaviors (cloud/local brain)

| # | Behavior | Trigger | AI task | Reaction |
|---|----------|---------|---------|----------|
| 5 | **Follow a person** | Camera → AI: "Is there a person? Where?" | Vision model returns position | Turn toward person, drive forward |
| 6 | **Recognize gestures** | Camera → AI: "What gesture?" | Vision model classifies | Wave→wag, point→go that direction, palm→stop |
| 7 | **Voice commands** | Mic → Whisper → text → AI | Speech-to-text + intent | "Sit", "Come", "Speak", "Stay" |
| 8 | **Learned preferences** *(stretch goal)* | Track interaction frequency | Simple reinforcement | Approaches people who pet it more, avoids loud noises |

---

## Software Architecture

```
wolf_puppy/
├── ROADMAP.md                  # This file
├── README.md
├── notebooks/                  # Jupyter notebooks for learning
│   ├── 01_hello_led.ipynb      # First program: blink an LED
│   ├── 02_servo_basics.ipynb   # Control a servo
│   ├── 03_motors.ipynb         # Drive motors forward/backward
│   ├── 04_sensors.ipynb        # Read ultrasonic + touch
│   ├── 05_camera.ipynb         # Take a photo, display it
│   ├── 06_ai_vision.ipynb      # Send photo to AI, get response
│   └── 07_ai_voice.ipynb       # Record audio, get transcription
├── sasha/                      # Main puppy code (runs on Pi)
│   ├── main.py                 # Entry point: event loop
│   ├── config.py               # Pin assignments, thresholds, settings
│   ├── hardware/
│   │   ├── motors.py           # DC motor control (forward, turn, stop)
│   │   ├── servos.py           # Servo control (tail, head)
│   │   ├── sensors.py          # Ultrasonic + touch reading
│   │   ├── eyes.py             # NeoPixel ring patterns
│   │   └── speaker.py          # Sound playback
│   ├── brain/
│   │   ├── ai_client.py        # Abstract AI interface
│   │   ├── ollama_provider.py  # Ollama implementation
│   │   ├── openai_provider.py  # OpenAI implementation
│   │   ├── vision.py           # Camera capture → AI → parse result
│   │   └── voice.py            # Mic record → Whisper → text
│   ├── behavior/
│   │   ├── engine.py           # Behavior priority engine
│   │   ├── mood.py             # Mood state machine (happy/curious/scared)
│   │   ├── rules.py            # If→then behaviors
│   │   └── learned.py          # Stretch: preference learning
│   └── sounds/                 # Audio files
│       ├── bark_happy.wav
│       ├── bark_curious.wav
│       ├── whimper.wav
│       └── growl.wav
├── server/                     # AI server (runs on MacBook)
│   ├── app.py                  # FastAPI server
│   ├── requirements.txt
│   └── config.py               # Model selection, ports
└── templates/                  # Printable cardboard templates
    ├── body.pdf
    ├── head.pdf
    ├── leg_front.pdf
    ├── leg_rear.pdf
    ├── tail.pdf
    └── assembly_guide.pdf
```

### Key Design Decisions

- **AI provider abstraction:** `ai_client.py` defines a simple interface (`analyze_image(image) → str`, `transcribe_audio(audio) → str`). Ollama and OpenAI are swappable backends.
- **Behavior priority engine:** Multiple behaviors can trigger simultaneously. Engine picks highest priority: obstacle avoidance > voice command > gesture response > mood-driven idle.
- **Mood state machine:** States: `happy`, `curious`, `scared`, `sleepy`. Transitions based on sensor events (petted→happy, loud noise→scared, idle→sleepy). Mood affects eye color, movement speed, and sound selection.

---

## Week-by-Week Execution Plan

### Week 0 — Preparation (before the project starts)

**Who:** Parent only
**Time:** ~1 hour

- [ ] Order all parts from shopping list
- [ ] Flash Raspberry Pi OS Lite (64-bit) onto MicroSD card
- [ ] Boot Pi Zero 2 W, configure WiFi, enable SSH and camera
- [ ] Install Ollama on MacBook: `brew install ollama`
- [ ] Pull the vision model: `ollama pull llama3.2-vision:11b`
- [ ] Install Whisper: `pip install openai-whisper`
- [ ] Set up the Python dev environment on MacBook for Jupyter notebooks
- [ ] Print cardboard templates (body, head, legs, tail)

**Checkpoint:** Pi is accessible via SSH, Ollama answers a test prompt, templates are printed.

---

### Week 1 — Meet the Parts + First Code (2–3 hours)

**Theme:** "Let's meet Sasha's body parts and write our first code!"

#### Session 1A: Hardware show-and-tell (30–45 min)
- [ ] Spread all parts on a table — name each one together
- [ ] Explain what each part does with real-world analogies:
  - Pi = brain, Camera = eyes, Mic = ears, Speaker = mouth
  - Motors = leg muscles, Servos = neck/tail muscles
  - Ultrasonic = whiskers (feeling distance), NeoPixels = eye color/mood
  - Batteries = food (energy)
- [ ] Daughter draws a puppy and labels where each part goes

#### Session 1B: Breadboard — first circuit (45–60 min)
- [ ] Set up breadboard with Pi Zero 2 W
- [ ] Jupyter notebook `01_hello_led.ipynb`: blink an LED
  - She types the code, you explain each line
  - Teach: "if button pressed → LED on" (first if→then!)
- [ ] Connect one SG90 servo to breadboard
- [ ] Jupyter notebook `02_servo_basics.ipynb`: make servo sweep
  - She picks angles: "Make the tail go HERE... now HERE!"

#### Session 1C: Motors (30–45 min)
- [ ] Wire DRV8833 motor driver + one DC motor on breadboard
- [ ] Jupyter notebook `03_motors.ipynb`: motor forward, backward, speed control
- [ ] Add second motor, demonstrate differential steering
- [ ] "Drive around the table" game — she types commands to steer

**Checkpoint:** She can make a servo move to angles she chooses, and drive two motors with Python commands.

---

### Week 2 — Senses + AI Magic (2–3 hours)

**Theme:** "Teaching Sasha to see, hear, and feel!"

#### Session 2A: Sensors (45–60 min)
- [ ] Wire ultrasonic sensor (HC-SR04) on breadboard
- [ ] Jupyter notebook `04_sensors.ipynb`: read distance, print to screen
- [ ] Game: "How far is my hand?" — she moves hand, reads the number
- [ ] Wire touch button on breadboard
- [ ] Code: "if button pressed → servo wags" (first real behavior!)

#### Session 2B: Camera + AI Vision (60–75 min)
- [ ] Connect Pi Camera Module 2
- [ ] Jupyter notebook `05_camera.ipynb`: take a photo, display it
- [ ] Start Ollama AI server on MacBook (parent sets up FastAPI server)
- [ ] Jupyter notebook `06_ai_vision.ipynb`:
  - Take photo → send to AI → get response
  - "Sasha, what do you see?" — AI describes the image
  - Stand in front of camera: "Is there a person?" → AI: "Yes!"
  - Try different gestures: wave, thumbs up, point
- [ ] Discussion: "You wrote the code for the button (if→then). But who wrote the code that recognizes your face? Nobody! The AI learned it from millions of photos. That's the difference!"

#### Session 2C: Voice (30–45 min)
- [ ] Plug in USB microphone
- [ ] Jupyter notebook `07_ai_voice.ipynb`: record 3 seconds → transcribe
- [ ] Say commands: "Sit!" "Come!" "Speak!" — see them as text
- [ ] Simple if→then on transcribed text: `if "sit" in text: servo.angle = 0`

**Checkpoint:** She has coded: button→wag, obstacle detection, camera→AI→response, voice→text→action. She understands if→then vs. AI.

---

### Week 3 — Building Sasha's Body (2–3 hours)

**Theme:** "Let's build Sasha's body and put everything inside!"

#### Session 3A: Cut and fold (60–75 min)
- [ ] Cut cardboard templates together (she cuts, you help with tricky parts)
- [ ] Fold along template lines, glue tabs
- [ ] Reinforce legs with popsicle sticks inside
- [ ] Test-fit motors into rear legs, mark mount points
- [ ] Test-fit servos for tail and head pivot points
- [ ] Cut holes: camera lens (face), ultrasonic sensor (nose), speaker (mouth)

#### Session 3B: Assemble mechanics (45–60 min)
- [ ] Hot-glue motors into rear leg mounts (parent operates glue gun)
- [ ] Attach wheels to motor shafts (rear legs)
- [ ] Attach free-spinning wheels to front legs
- [ ] Mount tail servo at rear of body, attach cardboard tail arm
- [ ] Mount head tilt + turn servos at neck joint
- [ ] Verify: all moving parts have clearance, wheels touch ground evenly

#### Session 3C: Wire and mount electronics (30–45 min)
- [ ] Mount Pi Zero 2 W inside body (velcro or cardboard bracket)
- [ ] Mount camera module in face hole
- [ ] Mount NeoPixel rings in eye holes (hot glue from inside)
- [ ] Mount ultrasonic sensor in nose position
- [ ] Mount touch button on top of head
- [ ] Mount speaker inside body near mouth opening
- [ ] Route all wires neatly, zip-tie bundles
- [ ] Mount battery packs (bottom of body, accessible for replacement)

**Checkpoint:** Sasha is physically assembled. Wheels spin, tail wags, head moves, eyes glow when powered on.

---

### Week 4 — Bringing Sasha to Life (2–3 hours)

**Theme:** "Let's give Sasha her personality!"

#### Session 4A: Basic behaviors (60–75 min)
- [ ] Transfer working notebook code into `sasha/` module structure
- [ ] Implement `main.py` event loop
- [ ] Wire up if→then behaviors:
  - Touch → tail wag + happy bark + green eyes
  - Obstacle < 15cm → stop, back up, turn, yellow eyes
  - Loud sound → head tilt + curious bark + blue flash
- [ ] Test each behavior — she triggers them, they react

#### Session 4B: AI behaviors (45–60 min)
- [ ] Implement person following:
  - Camera → AI: "Is there a person? Are they left, center, or right?"
  - Left → turn left, center → drive forward, right → turn right
- [ ] Implement gesture recognition:
  - Wave → tail wag
  - Point right → turn right and go
  - Palm out → stop
- [ ] Implement voice commands:
  - "Sit" → servos to sit position
  - "Come" → drive toward voice (forward)
  - "Speak" → play bark sound

#### Session 4C: Mood system (30–45 min)
- [ ] Implement mood state machine:
  - `happy`: green eyes, faster movement, frequent tail wags
  - `curious`: blue eyes, head tilting, slower movement
  - `scared`: red eyes, backing away, whimper sounds
  - `sleepy`: dim purple eyes, minimal movement, yawn sound
- [ ] Transitions: petted→happy, loud noise→scared, idle 30s→sleepy, new person→curious
- [ ] She picks which moods map to which eye colors and sounds

**Checkpoint:** Sasha has personality! She responds to touch, obstacles, sounds, gestures, voice, and has moods.

---

### Week 5 — Polish + Stretch Goals (2–3 hours)

**Theme:** "Making Sasha even smarter and prettier!"

#### Session 5A: Sound design + decoration (45–60 min)
- [ ] Record or download bark/whimper/growl sound effects together
- [ ] She picks which sound goes with which behavior
- [ ] Add text-to-speech: Sasha can "say" things ("I'm happy!", "What was that?")
- [ ] Decorate! Paint, draw, stickers — make Sasha beautiful
- [ ] Add name tag

#### Session 5B: Stretch — Learned preferences (45–60 min) *(if time allows)*
- [ ] Explain concept: "Sasha will remember what she likes!"
- [ ] Simple tracking: count how often each person pets her
- [ ] Behavior: approach frequent-petter more eagerly (faster, more wags)
- [ ] Track: loud noise frequency → if many, become more skittish
- [ ] Store preferences in a simple JSON file on Pi

#### Session 5C: Demo day! (30–45 min)
- [ ] Test all behaviors end-to-end
- [ ] Fix any remaining issues
- [ ] She demonstrates Sasha to family/friends
- [ ] Discussion: "What would you add next?" — write ideas in a FUTURE.md

**Checkpoint:** Sasha is complete! All behaviors work, she's decorated, and ready to show off.

---

## Wiring Diagram (Breadboard Phase)

```
                    Raspberry Pi Zero 2 W
                 ┌──────────────────────────┐
                 │  5V ──────┬──────────────│──── Servo VCC (all 3)
                 │  GND ─────┼──────────────│──── Common ground
                 │           │              │
    USB Mic ────▶│  USB      │              │
                 │           │              │
  Pi Camera ────▶│  Camera   │              │
                 │  port     │              │
                 │           │              │
                 │  GPIO17 ──│──── Servo 1 (tail) signal
                 │  GPIO27 ──│──── Servo 2 (head tilt) signal
                 │  GPIO22 ──│──── Servo 3 (head turn) signal
                 │           │              │
                 │  GPIO12 ──│──── DRV8833 IN1 (left motor)
                 │  GPIO13 ──│──── DRV8833 IN2 (left motor)
                 │  GPIO18 ──│──── DRV8833 IN3 (right motor)
                 │  GPIO19 ──│──── DRV8833 IN4 (right motor)
                 │           │              │
                 │  GPIO23 ──│──── HC-SR04 TRIG
                 │  GPIO24 ◄─│──── HC-SR04 ECHO (via voltage divider*)
                 │           │              │
                 │  GPIO25 ◄─│──── Touch button (with 10kΩ pull-down)
                 │           │              │
                 │  GPIO10 ──│──── NeoPixel DATA IN (chain both rings)
                 │           │              │
                 │  PWM Audio│──── PAM8403 input → Speaker
                 └───────────┘
                     │
                USB power bank

    4×AA Battery Pack ──── DRV8833 VCC
                      ──── Servo VCC (shared rail)
                      ──── NeoPixel VCC
                      ──── Common GND

    * HC-SR04 ECHO is 5V; Pi GPIO is 3.3V.
      Use a voltage divider (1kΩ + 2kΩ) to bring ECHO down to 3.3V.
```

---

## Risk Register

| Risk | Impact | Mitigation |
|------|--------|------------|
| Cardboard body too flimsy for motors | Puppy falls apart | Reinforce with popsicle sticks and extra glue layers |
| Motor vibration loosens wires | Intermittent failures | Zip-tie all connections, hot glue wire junctions |
| Pi Zero 2 W underpowered for camera + WiFi | Slow/laggy | Reduce camera resolution to 640×480, optimize capture interval |
| AI response too slow (>3 sec) | Puppy feels unresponsive | Cache frequent responses, use smaller Ollama model if needed |
| Daughter loses interest mid-project | Project abandoned | Each week produces a working milestone — celebrate each one! |
| Parts don't arrive in time | Week 1 delayed | Order in Week 0 with buffer time, have backup sources |
| 4×AA batteries drain fast with motors | Short play sessions | Use rechargeable AAs, keep spare set charged |
| Learned preferences too complex for timeline | Week 5 stress | Marked as stretch goal — skip without guilt |

---

## Quick Reference

| What | Command/Action |
|------|---------------|
| SSH into Pi | `ssh pi@sasha.local` |
| Start AI server (MacBook) | `cd server && uvicorn app:app --host 0.0.0.0 --port 8000` |
| Run Sasha (on Pi) | `cd ~/sasha && python main.py` |
| Test single servo | `python -c "from hardware.servos import tail; tail.wag()"` |
| Test motors | `python -c "from hardware.motors import drive; drive.forward(0.5)"` |
| Check camera | `libcamera-still -o test.jpg` |
| View logs | `tail -f ~/sasha/sasha.log` |
