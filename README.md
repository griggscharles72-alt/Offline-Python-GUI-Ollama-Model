# Offline-Python-GUI-Ollama-ModelGood.

We reset clean.

No WebUI.
No Docker debugging.
No bridge routing.
No VPN complications.

We are building:

> **Offline Python GUI → Ollama → Model**
> Run from IDLE (F5) or `python3 brainchat.py`

That’s it.

---

## Step 1 — Confirm Ollama is alive

Run:

```bash
ollama list
```

If you see:

* llama3:8b
* phi3
* mistral

We’re good.

---

## Step 2 — Make the GUI (single file)

Paste this exactly:

```bash
mkdir -p ~/brain && cd ~/brain
nano brainchat.py
```

Paste this into the file:

```python
#!/usr/bin/env python3
import requests
import threading
import tkinter as tk
from tkinter import ttk

OLLAMA_URL = "http://localhost:11434"
MODEL = "llama3:8b"

class ChatApp(tk.Tk):
    def __init__(self):
        super().__init__()
        self.title("BrainChat (Offline)")
        self.geometry("900x650")

        self.chat_history = []

        self.text = tk.Text(self, wrap="word", state="disabled")
        self.text.pack(fill="both", expand=True, padx=10, pady=10)

        self.entry = tk.Text(self, height=3)
        self.entry.pack(fill="x", padx=10)

        send_btn = ttk.Button(self, text="Send", command=self.send)
        send_btn.pack(pady=5)

        self.bind("<Control-Return>", lambda e: self.send())

    def append(self, role, message):
        self.text.configure(state="normal")
        self.text.insert("end", f"{role}: {message}\n\n")
        self.text.see("end")
        self.text.configure(state="disabled")

    def send(self):
        prompt = self.entry.get("1.0", "end").strip()
        if not prompt:
            return
        self.entry.delete("1.0", "end")
        self.append("You", prompt)

        messages = self.chat_history + [{"role": "user", "content": prompt}]

        def worker():
            try:
                r = requests.post(
                    f"{OLLAMA_URL}/api/chat",
                    json={"model": MODEL, "messages": messages, "stream": False},
                    timeout=120
                )
                r.raise_for_status()
                reply = r.json()["message"]["content"]
                self.chat_history.extend([
                    {"role": "user", "content": prompt},
                    {"role": "assistant", "content": reply}
                ])
                self.after(0, lambda: self.append("Brain", reply))
            except Exception as e:
                self.after(0, lambda: self.append("Error", str(e)))

        threading.Thread(target=worker, daemon=True).start()

if __name__ == "__main__":
    app = ChatApp()
    app.mainloop()
```

Save:

* Ctrl+O
* Enter
* Ctrl+X

---

## Step 3 — Install dependency

```bash
python3 -m pip install --user requests
```

---

## Step 4 — Run it

From terminal:

```bash
python3 brainchat.py
```

OR open it in IDLE and hit **F5**.

---

That’s it.

You now have:

* A real window
* Offline
* No browser
* No Docker
* No VPN dependency
* No IP routing drama

---

If it throws any error, paste it and we fix only that line.

This is the stable path.
