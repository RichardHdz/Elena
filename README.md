# Project Elena

> A self-hosted, Jarvis-like AI assistant built on a custom Python stack — no Ollama, no LangChain, just direct inference and hand-rolled agent logic.

---

## What Is Elena?

Elena is a personal AI assistant designed for full local control, long-term memory, and physical device integration. The goal is a system that knows its operator, remembers context across sessions, and can interact with the real world — not just answer questions in a chat window.

This is a personal project in active development. Architecture decisions are intentional and opinionated.

---

## Core Design Goals

- **No framework lock-in** — inference runs via `llama-cpp-python` directly; agent loops are hand-rolled
- **Persistent memory** — Obsidian vault serves as the shared long-term memory layer, accessible to both Elena (via Python file access) and Claude (via MCP)
- **Physical world integration** — 3D printer, smart home devices, and other hardware via REST APIs
- **Local-first** — designed to run on self-hosted hardware with no required cloud dependencies

---

## Architecture Overview

```
┌─────────────────────────────────────────────────┐
│                  Elena Core                     │
│                                                 │
│   ┌─────────────┐     ┌─────────────────────┐   │
│   │  Inference  │     │    Agent Loop       │   │
│   │  llama-cpp  │────▶│  (hand-rolled)      │   │
│   │  -python    │     │                     │   │
│   └─────────────┘     └────────┬────────────┘   │
│                                │                │
│              ┌─────────────────┼──────────────┐ │
│              ▼                 ▼              ▼ │
│   ┌──────────────┐  ┌──────────────┐  ┌──────────────┐ │
│   │  Obsidian    │  │  REST API    │  │  Other       │ │
│   │  Vault       │  │  Integrations│  │  Tools       │ │
│   │  (Memory)    │  │  (Devices)   │  │              │ │
│   └──────────────┘  └──────────────┘  └──────────────┘ │
└─────────────────────────────────────────────────┘
```

---

## Hardware Target

Long-term platform: **Refurbished Dell OptiPlex Full Tower (7060 / 7070 / 7080)**

Chosen for:
- GPU upgrade headroom (full-size PCIe slot)
- Low cost of entry
- Quiet, stable operation
- Easy component access

A temporary machine is used for the exploratory phase while hardware is finalized.

---

## Memory Layer

Elena uses an **Obsidian vault** as her persistent memory store. The vault lives on an external drive connected to the development machine and is structured with seeded notes covering all active projects and operator context.

- Vault path (dev): `/Volumes/Storage1/ObsidianVault/MainAIVault`
- Elena accesses the vault via direct Python file I/O
- Claude can access the same vault via MCP (Cowork interface)

This shared memory design means both systems stay in sync without duplication.

---

## Planned Integrations

| Integration | Method | Status |
|---|---|---|
| 3D Printer | REST API | Planned |
| Smart Home Devices | REST API | Planned |
| Obsidian Vault | Python file access | Active |
| Claude (MCP bridge) | MCP / Cowork | Active |

---

## Project Status

**Phase: Exploratory / Early Development**

- [x] Architecture defined
- [x] `.gitignore` configured
- [x] Obsidian vault seeded and structured
- [x] MCP bridge to Claude via Cowork established
- [ ] llama-cpp-python inference layer
- [ ] Hand-rolled agent loop (v1)
- [ ] REST API device integrations
- [ ] Long-term memory read/write via vault

---

## Repository Structure

```
~/projects/elena/
├── README.md
├── .gitignore
├── brain/              # LLM (Ollama) interaction code
├── voice/              # Whisper STT + Piper TTS code
├── agent/              # LangChain agent core
├── memory/             # SQLite + ChromaDB
├── tools/              # Individual tools Elena can call
├── config/             # Settings, API keys, personality prompt
└── logs/               # Runtime logs for debugging
```

> Note: `.env`, secrets, model weights, and large binary files are excluded via `.gitignore`.

---

## Philosophy

Elena is built to be understood completely by the person running her. Every component is chosen because it can be read, modified, and replaced — not because it was the fastest path to a working demo.

---

## License

Personal project. Not licensed for redistribution at this time.
