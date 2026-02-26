# BrainChat (Offline Python GUI)

Generated: 2026-02-26T06:10:35.140370 UTC

## What This Is

BrainChat is a local offline GUI chat application built in Python.
It connects directly to Ollama running on localhost.

Architecture:

Python (Tkinter GUI)
    ↓
http://localhost:11434
    ↓
Ollama Model (llama3:8b by default)

No Docker.
No WebUI.
No browser.
No external API.

This is your own local interface layer talking to your local model.

---

## Requirements

- Linux
- Python 3
- Ollama installed and running
- Model pulled (example: llama3:8b)
- requests Python package

---

## Install

pip install requests

---

## Run

python3 brainchat.py

Or open in IDLE and press F5.

---

## Change Model

Edit this line in brainchat.py:

MODEL = "llama3:8b"

Replace with:
- "phi3:latest"
- "mistral:latest"
- or any model returned by: ollama list

---

## Notes

- Chat history persists during session only.
- Completely offline after model download.
- Safe from VPN/IP changer interference as long as Ollama runs on localhost.
