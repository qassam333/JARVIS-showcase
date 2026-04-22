# JARVIS - Local Second Brain Assistant

> **Privacy-First • Deterministic • Fully Local**

A modular AI assistant inspired by Iron Man's JARVIS. Manages tasks, schedules, knowledge, and integrates with university systems. **No cloud. No telemetry. My data is mine**

---

## Tech Stack

| Component | Technology | Reason |
|-----------|------------|--------|
| **Language** | Python 3.11+ | Rich ecosystem, fast development |
| **Database** | SQLite | Local, portable, encrypted at rest (V2) |
| **CLI** | Rich + Click | Beautiful terminal output |
| **Voice STT** | Whisper.cpp | 100% local, no cloud |
| **Voice TTS** | Piper | Local TTS |
| **Web Scraping** | httpx + BeautifulSoup4 | Async, lightweight |
| **API** | FastAPI | REST API for web UI |
| **Web UI** | React + Vite + Tailwind | Modern Dashboard Interface |

## Latest Updates (v0.2.0)

| # | Feature | Description |
|---|---------|-------------|
| 1 | **Daily Task Selection** | Auto-selects daily tasks based on priority + deadline urgency |
| 2 | **Goal → Tasks Generator** | Breaks goals into actionable phase-based tasks |
| 3 | **Sequential Unlocking** | Next task unlocks only when previous is done |
| 4 | **Auto-Rollover** | Undone tasks automatically move to next day |
| 5 | **Daily Tasks Table** | Tracks daily task selection with scoring |

---

## Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                          USER INTERFACE                     │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐  ┌─────┐ │
│  │  CLI/Text   │  │  Voice STT  │  │  REST API   │  │Wake │ │
│  └─────────────┘  └─────────────┘  └─────────────┘  └─────┘ │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│                           CORE BRAIN                        │
│  ┌──────────────┐    ┌──────────────┐    ┌──────────────┐   │
│  │ Intent       │──▶│ Memory       │──▶│ Decision     │   │
│  │ Parser       │    │ Engine       │    │ Engine       │   │
│  └──────────────┘    └──────────────┘    └──────────────┘   │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│                    SKILLS (Actions)                         │
│  ┌───────────┐  ┌───────────┐  ┌───────────┐  ┌─────────┐   │
│  │  Tasks    │  │  Notes    │  │ Knowledge │  │Schedule │   │
│  └───────────┘  └───────────┘  └───────────┘  └─────────┘   │
│  ┌───────────────────────────────────────────────────────┐  │
│  │          UNIVERSITY SCRAPER (Moodle)                  │  │
│  └───────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│                        PERSISTENCE LAYER                    │
│  ┌──────────────────────────────────────────────────────┐   │
│  │   SQLite + Migration System + Encrypted Credentials  │   │
│  └──────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────┘
```

---

## Mental Model

```
[ INPUT ] → [ INTERPRETATION ] → [ DECISION ] → [ ACTION ]
```

**Build**: INTERPRETATION contract + DECISION engine

---

## Project Structure

```
JARVIS/
├── jarvis/                      # Main package
│   ├── __init__.py
│   ├── main.py                 # Entry point
│   │
│   ├── core/                   # Core brain
│   │   ├── brain.py
│   │   ├── memory.py
│   │   └── intent_parser.py
│   │
│   ├── dashboard/              # Web Dashboard Interface
│   │   ├── frontend/           # React + Vite
│   │   └── backend/            # FastAPI Server
│   │
│   ├── db/                     # Database layer
│   │   ├── database.py         # SQLite connection
│   │   ├── migrations/         # Migration system
│   │   │   ├── __init__.py
│   │   │   └── scripts/
│   │   │       └── __init__.py
│   │   └── encryption.py      # (V2)
│   │
│   ├── skills/                 # Skills/actions
│   │   ├── tasks.py
│   │   ├── notes.py
│   │   ├── knowledge.py
│   │   ├── schedule.py
│   │   └── university/
│   │
│   ├── voice/                  # Voice interface (DONE)
│   │   ├── __init__.py
│   │   ├── audio.py           # Audio capture utilities
│   │   ├── stt.py             # Whisper STT
│   │   ├── tts.py             # Piper TTS
│   │   ├── wake_word.py      # Wake word detection
│   │   └── voice_cli.py      # Main voice loop
│   │
│   ├── api/                    # Core API hooks
│   │   ├── __init__.py
│   │   └── health.py
│   │
│   └── utils/                  # Utilities
│       ├── __init__.py
│       ├── config.py           # Config + env vars
│       └── logger.py           # Structured logging
│
├── tests/                      # Unit tests
├── data/                      # SQLite DB location
│
├── config.yaml                 # Configuration
├── pyproject.toml              # Dependencies
├── .gitignore                  # Git ignore
├── docs/                       # Documentation
│   ├── ARCHITECTURE.md
│   ├── DATABASE.md
│   ├── SKILLS.md
│   ├── UNIVERSITY_INTEGRATION.md
│   ├── VOICE_INTERFACE.md
│   ├── CLI_REFERENCE.md
│   ├── QUICK_START.md
│   ├── MIGRATIONS.md           # NEW
│   ├── LOGGING.md              # NEW
│   ├── API_LAYER.md            # NEW
│   ├── CONFIGURATION.md        # NEW
│   ├── ENVIRONMENT.md          # NEW
│   └── DEPENDENCIES.md         # NEW
│
└── PROJECT_SUMMARY.md          # This file
```

---

## Privacy Guarantees

| Guarantee | Implementation |
|-----------|----------------|
| Zero network calls | No external APIs required |
| No telemetry | Explicitly excluded |
| No cloud | 100% local operation |
| Open source | Full auditable code |
| Secure credentials | Encrypted in V2 |
| Rate limited | Respect servers when scraping |

---

## Getting Started

```bash
# Install
poetry install

# Initialize database
poetry run python -m jarvis.main

# Run CLI
poetry run jarvis --help
```

---

## Dependencies

```toml
python = "^3.11"
pydantic = "^2.0"
pydantic-settings = "^2.0"
sqlalchemy = "^2.0"
cryptography = "^41.0"
pyyaml = "^6.0"
rich = "^13.0"
click = "^8.1"
httpx = "^0.25"
beautifulsoup4 = "^4.12"
```

---

<div align="center">

**Not "Jarvis the Fantasy"**

**But Jarvis that Actually Works.**

</div>
