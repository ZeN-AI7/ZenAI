# ZenAI
## A prototype of a very potential and active agent that can do tasks for you while you can have your most desired coffee break!

ZenFlow-Agentic-AI/
├── agent.py
├── main.py
├── .env.example
├── .gitignore
├── memory.json  # (ignored in Git)
├── requirements.txt
├── README.md
├── tasks/
│   ├── __init__.py
│   ├── create_file.py
│   ├── read_file.py
│   └── open_url.py
└── utils/
    └── logger.py

# Tasks/create_file.py
import os

def run(file_name: str = "default.txt") -> str:
    directory = os.path.dirname(file_name)
    if directory and not os.path.exists(directory):
        os.makedirs(directory)

    with open(file_name, "w") as f:
        f.write("This file was created by ZenFlow agent.\n")
    return f" File '{file_name}' created successfully."

# Tasks/read_file.py
import os

def run(file_name: str = "default.txt") -> str:
    if not os.path.isfile(file_name):
        return f"File '{file_name}' not found."
    try:
        with open(file_name, "r") as f:
            content = f.read()
        return f"📄 Contents of '{file_name}':\n\n{content}"
    except Exception as e:
        return f"Error reading file '{file_name}': {str(e)}"

# Tasks/open_url.py
import webbrowser

def run(url: str = "https://www.google.com") -> str:
    if not url.startswith("http"):
        url = "https://" + url
    try:
        webbrowser.open(url)
        return f"Opening {url} in your default browser."
    except Exception as e:
        return f"Failed to open URL: {str(e)}"

# Tasks/__init__.py
### empty file

# Agent.py
import os
from dotenv import load_dotenv
from langchain.agents import initialize_agent, Tool
from langchain_google_genai import GoogleGenerativeAI
from tasks import create_file, read_file, open_url

load_dotenv()

llm = GoogleGenerativeAI(model="gemini-pro", temperature=0)

tools = [
    Tool(name="CreateFile", func=create_file.run, description="Creates a file"),
    Tool(name="ReadFile", func=read_file.run, description="Reads contents of a file"),
    Tool(name="OpenURL", func=open_url.run, description="Opens a website in browser"),
]

agent = initialize_agent(tools, llm, agent="zero-shot-react-description", verbose=True)

def zen_agent(command):
    return agent.run(command)

# Main.py
from agent import zen_agent

if __name__ == "__main__":
    while True:
        user_input = input("\n🤖 ZenFlow > ")
        if user_input.lower() in ["exit", "quit"]:
            break
        try:
            response = zen_agent(user_input)
            print(f"\n{response}\n")
        except Exception as e:
            print(f"\nError: {str(e)}\n")

# .env.example 
## Copy this file to .env and add your Google API key
GOOGLE_API_KEY=your_api_key_here

# .gitignore
.env
memory.json
__pycache__/
.venv/

# Requirements.txt
langchain==0.1.20
langchain-google-genai==0.0.9
google-generativeai==0.3.2
python-dotenv

# README.md 
# 🤖 ZenFlow: Your First Agentic AI Task Automator

ZenFlow is a beginner-friendly, fully local AI agent that understands your natural language commands and automates basic tasks like creating files, reading file content, and opening websites.

## 🌟 Features
- LangChain + Gemini-powered Agent
- Natural language command execution
- Modular tool system (easy to extend)
- Local execution, no cloud deployment needed

## ⚙️ Example Commands
```
Create a file named tasks/plan.md
Read file tasks/plan.md
Open github.com
```

## 🚀 Getting Started
1. Clone the repo:
```bash
git clone https://github.com/yourusername/ZenFlow-Agentic-AI.git
cd ZenFlow-Agentic-AI
```

2. Install dependencies:
```bash
pip install -r requirements.txt
```

3. Add your Gemini API key:
```bash
cp .env.example .env
# Edit the .env file and paste your GOOGLE_API_KEY
```

4. Run ZenFlow:
```bash
python main.py
```

## 🛠️ Architecture
```
[User Command]
      ↓
   [LLM Agent] ← LangChain + Gemini
      ↓
[Tool Handler] → create_file.py / read_file.py / open_url.py
      ↓
   [Result Shown + (Optional: Logged)]
```

## 📁 Tools You Can Build Next
- Shell Command Executor
- Voice Assistant Mode
- Memory Logger (JSON / SQLite)
- LangGraph Migration (Advanced Agents)

## 🧠 Author
Built by [@DzenAutomations](https://linkedin.com/in/https://www.linkedin.com/in/debbhowmik/) to kickstart your Agentic AI journey.

## ⭐ Star it if it helped you, fork it to build your own workspace assistant!
