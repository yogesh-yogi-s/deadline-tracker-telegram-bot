# 📅 Deadline Tracker — Telegram AI Vision App

> **Snap your syllabus, timetable, or assignment sheet. Track your upcoming deadlines. Receive a structured reminder digest straight on Telegram.**

Built with **Streamlit**, **Google Gemini Vision (Chat & Multimodal AI)**, and **Telegram Bot API**.

---

## 🌟 Overview

Students and professionals often juggle multiple deadlines across syllabi, assignment PDFs, lecture slides, exam schedules, and messy whiteboard notices.

**Deadline Tracker** solves this by letting you:
1. **Onboard once** with your name and Telegram Chat ID.
2. **Snap or upload a photo** of any syllabus, timetable, assignment sheet, or notice (or type free-form text).
3. **Analyze with Gemini Vision**: The AI extracts dates, deliverables, course codes, submission criteria, and urgency levels while maintaining conversational memory.
4. **One-click Telegram reminder digest**: Tap **"📤 Send to Telegram"** to get a clean, chronologically sorted, emoji-enhanced deadline digest sent straight to your phone.

---

## 🏗️ Architecture Flow

```text
USER
  ↓
ONBOARDING (Name + Telegram Chat ID)
  ↓
STREAMLIT CHAT INTERFACE
  ↓
TEXT / IMAGE (Syllabus, Timetable, Assignment photo)
  ↓
GEMINI 3.5 FLASH (Vision & Multimodal Reasoning)
  ↓
CONVERSATION MEMORY (Stateful Chat Session)
  ↓
DEADLINE EXTRACTION & RECAP
  ↓
TELEGRAM BOT API (Direct to User's Chat)
```

---

## 🚀 Key Features

* **📷 Multimodal Vision Extraction**: Hand raw images of course schedules or assignment prompts directly to Gemini without OCR pre-processing.
* **🧠 Persistent Context & Memory**: Chat session maintains state so you can ask follow-up questions (e.g., *"Which of these assignments has the highest weightage?"* or *"What is due next Monday?"*).
* **⚡ Graceful Ambiguity Handling**: If dates are cut off or unclear, the assistant explicitly flags uncertainties rather than hallucinating dates.
* **📲 Instant Telegram Reminders**: Automatically compiles all discussed deadlines into a clean mobile-friendly format and dispatches via Telegram Bot.
* **🛡️ Production Security**: API keys and tokens are managed via `.streamlit/secrets.toml` and kept out of Git version control.

---

## 📁 Project Structure

```text
├── app.py                      # Core Streamlit application & Telegram dispatch logic
├── prompts.py                  # Scoped system instructions, templates & reminder prompts
├── test_app.py                 # Automated unit and AppTest integration test suite
├── requirements.txt            # Python dependencies (streamlit, google-genai, python-telegram-bot)
├── .gitignore                  # Prevents secrets.toml, virtual environments, & cache from being tracked
├── README.md                   # Documentation & setup guide
└── .streamlit/
    ├── secrets.toml.example    # Configuration template for secrets
    └── secrets.toml            # (Local only - NOT committed to Git)
```

---

## ⚙️ Prerequisites & Setup Guide

### 1. Requirements
* Python 3.9 or newer (tested on Python 3.11 - 3.13)
* Google AI Studio account ([aistudio.google.com](https://aistudio.google.com/))
* Telegram account and a Telegram Bot token from [@BotFather](https://t.me/BotFather)

### 2. Clone / Setup Workspace
```bash
git clone <your-repo-url>
cd "Telegram bot Deadline Tracker Nxtwave"
```

### 3. Create & Activate Virtual Environment
* **Windows (PowerShell)**:
  ```powershell
  python -m venv venv
  Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass
  .\venv\Scripts\Activate.ps1
  ```
* **macOS / Linux**:
  ```bash
  python3 -m venv venv
  source venv/bin/activate
  ```

### 4. Install Dependencies
```bash
pip install -r requirements.txt
```

### 5. Configure Secrets (`.streamlit/secrets.toml`)
Copy the example template:
```bash
# Windows
copy .streamlit\secrets.toml.example .streamlit\secrets.toml

# macOS / Linux
cp .streamlit/secrets.toml.example .streamlit/secrets.toml
```

Edit `.streamlit/secrets.toml` with your real keys:
```toml
# Google Gemini API key from Google AI Studio
GEMINI_API_KEY = "your-gemini-api-key-here"

# Telegram Bot Token from @BotFather
TELEGRAM_BOT_TOKEN = "your-telegram-bot-token-here"

# Optional: Gemini model override (defaults to gemini-3.5-flash)
GEMINI_MODEL = "gemini-3.5-flash"
```

---

## 🤖 Obtaining API Keys & Telegram IDs

### A. Google Gemini API Key
1. Visit [Google AI Studio](https://aistudio.google.com/).
2. Click **Get API key** and generate a new key.
3. Paste it into `GEMINI_API_KEY`.

### B. Telegram Bot Token
1. Open Telegram and search for [@BotFather](https://t.me/BotFather).
2. Send `/newbot`, name your bot (e.g. `MyDeadlineBot`), and choose a username ending in `bot`.
3. Copy the HTTP API token provided by BotFather into `TELEGRAM_BOT_TOKEN`.

### C. Telegram Chat ID
1. Search for [@userinfobot](https://t.me/userinfobot) on Telegram and send `/start`.
2. It replies with your numeric **Id** (e.g., `123456789`).
3. **Important**: Also open your own bot in Telegram and tap **Start** (or send any greeting) once so Telegram authorizes the bot to message you!

---

## 💻 Running Locally

```bash
streamlit run app.py
```

The application opens automatically at `http://localhost:8501`.
1. Fill in your name and numeric Telegram Chat ID on the onboarding screen.
2. Click **"Let's go 🚀"**.
3. Upload a photo of your syllabus, assignment prompt, or schedule sheet (or type in questions).
4. Tap **"📤 Send to Telegram"** to get the digest delivered directly to your chat!

---

## 🧪 Running Automated Tests

Run the included test suite to verify prompt scoping, onboarding validation, error boundary behavior, and Telegram text truncation:

```bash
python -m unittest test_app.py
```

All 6 test cases run via Streamlit's official `AppTest` framework.

---

## ☁️ Deployment on Streamlit Community Cloud

1. Push your repository to GitHub (ensure `.streamlit/secrets.toml` is ignored by Git).
2. Go to [share.streamlit.io](https://share.streamlit.io/) and log in with GitHub.
3. Select your repository, branch (`main`), and set the main file path to `app.py`.
4. Under **Advanced Settings → Secrets**, paste the contents of your `secrets.toml`:
   ```toml
   GEMINI_API_KEY = "..."
   TELEGRAM_BOT_TOKEN = "..."
   GEMINI_MODEL = "gemini-3.5-flash"
   ```
5. Click **Deploy!** Your Deadline Tracker is live.

---

## 📜 Evaluation Criteria Checklist

- [x] **Core Flow & Functionality**: Onboarding $\to$ Multimodal Vision Chat $\to$ Memory $\to$ Telegram Dispatch.
- [x] **Scoped Prompt Engineering**: Specialized in syllabi, assignments, exams, and graceful failure.
- [x] **Code Quality & Architecture**: Caching with `@st.cache_resource`, async safe execution, unit tests included.
- [x] **Security**: Secrets kept strictly in `.streamlit/secrets.toml` and `.gitignore` properly configured.
