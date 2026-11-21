# Voice AI Coding Assistant (LangChain + LangGraph)

Talk to your computer. It listens, thinks with an AI (built with **LangGraph**), and speaks back.
In Phase 2 it can also **list, read, create and edit files** inside the `workspace/` folder — but only after you type **yes**.

```
Phase 1:  START -> chatbot -> END
Phase 2:  START -> assistant -> (needs a tool?) -> tools -> assistant -> END
```

## Quick start (Windows)
1. Install **Python 3.10 or newer** — 3.12, 3.13, 3.14 all work (tick *Add python.exe to PATH*).
2. Double-click **`setup.bat`** (one time).
3. Open **`.env`** and paste your real OpenAI API key.
4. Double-click **`run.bat`** and start talking. Say **"exit"** to stop.

## Test step by step
| Step | Command | What should happen |
|---|---|---|
| 1 Microphone | `python test_voice_input.py` | Your sentence is printed |
| 2 Speaker | `python test_voice_output.py` | The computer speaks |
| 3 Phase 1 AI (typing) | `python chat_graph.py` | Type a question, AI answers and remembers |
| 4 File tools (no AI) | `python file_tools.py` | Lists files, blocks `../secret.py` |
| 5 Phase 2 AI (typing) | `python coding_graph.py` | "create hello.py that prints hello" → preview → type yes |
| 6 Full voice app | `python main.py` | Talk to it! |

To run the voice app as the Phase 1 chatbot, change the import line at the top of `main.py` to `from chat_graph import ask_ai`.

## Files
| File | Job |
|---|---|
| `config.py` | Reads `.env` and creates the AI model (`ChatOpenAI`) |
| `voice_input.py` | Microphone → text |
| `voice_output.py` | Text → voice |
| `chat_graph.py` | Phase 1 LangGraph chatbot with memory |
| `file_tools.py` | Phase 2 safe tools (`@tool`) for the workspace only |
| `coding_graph.py` | Phase 2 LangGraph agent: AI + ToolNode |
| `main.py` | Voice loop: listen → ask_ai → speak |
| `workspace/` | The only folder the AI can touch |

## Try saying
- "What is a Python function?"
- "List the files in my workspace."
- "Create hello.py that prints Hello students."
- "Explain hello.py in simple words."
- "Add a second print line to hello.py."
- "Exit."

## Common problems
| Problem | Fix |
|---|---|
| `No suitable Python runtime found` | `py -3.12` needs exactly 3.12. Use `python -m venv .venv` instead — any Python 3.10+ works |
| `No module named ...` | Run `setup.bat` again (or activate `.venv` and `pip install -r requirements.txt`) |
| `Activate.ps1` not found in CMD | In Command Prompt use `.venv\Scripts\activate.bat` (Activate.ps1 is for PowerShell) |
| Microphone error | Windows Settings → Sound → Input: choose your mic, and allow apps to use the microphone (Privacy settings). Run `python list_microphones.py` and set `MICROPHONE_INDEX` in `voice_input.py` |
| Not understood | Speak right after "Listening..." and finish within 5 seconds (change `RECORD_SECONDS` for more time). Speak closer. Change `LANGUAGE` in `voice_input.py` (`en-IN`, `en-US`, `hi-IN`) |
| No sound | Check speakers/volume. Run `python test_voice_output.py` |
| "Please put your real OPENAI_API_KEY" | Edit `.env` — no spaces, no quotes |
| Model not found | Change `OPENAI_MODEL` in `.env` to a model your API account has |
| Quota / rate limit | API needs billing credits (ChatGPT subscription does not include API credits) |

Never share your `.env` file or your API key.
