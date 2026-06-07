# Chillax.AI

## Problem Statement

Legacy Python code can feel like a locked room.
Many teams inherit projects with minimal documentation, inconsistent structure, and hidden coupling between modules. That makes onboarding slow, bugs hard to trace, and refactoring risky.

This repo solves the real developer problem of understanding an existing Python codebase quickly without relying on cloud AI or losing control of your project data.

## Solution

Chillax.AI is an offline desktop IDE that brings code understanding, analysis, and explanation into one local app.

It combines:
- a project-aware file browser,
- a syntax-aware editor,
- a code relationship map,
- a local AI assistant powered by Ollama,
- and a FastAPI backend that parses Python code into structures the assistant can reason about.

Instead of opening files one by one and guessing how things connect, you can explore the architecture, inspect functions, and ask AI to explain behavior in plain English.

## What Makes This Different

This is not just another text editor.
It is built for the specific use case of:
- understanding unfamiliar legacy code,
- reviewing component interactions,
- explaining code intent,
- and doing it all offline with a local LLM.

That means the app stays usable in air-gapped environments and keeps sensitive code out of cloud services.

## Tech Stack

- Electron — desktop shell and native app experience
- React + Vite — fast frontend UI with responsive components
- Monaco Editor — powerful in-browser code editing and navigation
- FastAPI — backend API for file browsing, analysis, and AI prompts
- Python AST — static parsing of Python files into modules, functions, and imports
- Ollama — local large language model runner for code explanation and reasoning

## How This Was Built

The architecture was designed around three core flows:

1. **Project analysis**
   - The backend scans the sample project and builds an AST-based representation of files, classes, functions, and import relationships.
   - This model feeds the code map and helps the AI assistant understand the project structure.

2. **Developer exploration**
   - The frontend presents a file explorer, editor, and code map.
   - Clicking a file opens it instantly, and the code map shows how files and symbols are related.

3. **AI explanation**
   - The assistant can answer questions about selected code, describe what a function does, or summarize how a feature flows through the project.
   - Prompts are generated locally and sent to Ollama, so no cloud provider is involved.

## Impact

This project helps teams:

- onboard faster by turning opaque legacy code into understandable structure
- reduce review time by surfacing relevant files and relationships
- improve confidence before refactoring or fixing bugs
- keep sensitive code offline while still using modern AI-assisted development

## How to Run It

### Prerequisites

- Node.js 18+
- Python 3.11+ recommended
- Ollama installed and a model pulled locally

### Start the app

Option A — one-click:

1. Open `start.cmd`
2. Let it install the frontend/backend dependencies
3. It will launch the backend, frontend, and Electron app automatically

Option B — manual:

```powershell
cd d:\fyeshi\project\IDE

# Install backend dependencies
py -3.11 -m pip install -r backend/requirements.txt

# Install frontend dependencies
cd frontend
npm install
cd ..

# Install Electron dependencies
npm install

# Start Ollama in a separate terminal
ollama serve

# Start backend
cd backend
py -3.11 -m uvicorn main:app --port 8000 --reload

# Start frontend in another terminal
cd frontend
npm run dev

# Launch Electron from repo root
cd ..
npx electron .
```

Option C — all-in-one development mode:

```powershell
npm run electron:dev
```

## Notes

- Use `py -3.11` if your default `python` command points to a different or broken interpreter.
- The local AI assistant requires Ollama to be running.
- The included sample project is a small demo app used to validate code analysis and UI behavior.

## Project Structure

```
IDE/
├── electron/
│   └── main.js              # Electron main process
├── backend/
│   ├── main.py              # FastAPI server
│   ├── analyzer.py          # Python AST analysis and graph
│   ├── ollama_client.py     # Local Ollama integration
│   ├── requirements.txt
│   └── sample_project/      # Demo legacy Python codebase
├── frontend/
│   ├── src/
│   │   ├── App.jsx          # Main app shell and layout
│   │   ├── components/      # UI panels, editor, assistant, map
│   │   ├── api/client.js    # Backend API helpers
│   │   └── index.css        # Theme and styling
│   ├── package.json
│   └── vite.config.js
├── package.json             # Root scripts and Electron integration
├── start.cmd                # One-click startup script
└── README.md
```

