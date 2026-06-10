# Social-Media-Post-Analyzer
> **AI-powered social media post analyzer that extracts tone, intent, communication style, and a concise summary — built with LangChain, Gemini 2.5 Flash Lite, and Pydantic-validated structured output.**

---

## What This Is

A Streamlit web app that accepts any social media post as input and returns a structured four-dimensional analysis: emotional tone, primary intent, communication style, and a one-sentence summary. The pipeline is built on a LangChain LCEL chain (`prompt | llm | parser`) with a `PydanticOutputParser` ensuring type-safe, schema-validated responses every time — no fragile string parsing.

---

## Why This

Raw text from social media is noisy and ambiguous. This project demonstrates how to wrap a large language model in a production-like pipeline with structured output contracts, making results reliable enough to feed downstream systems (dashboards, CRMs, moderation tools). It also showcases clean separation of concerns across prompt engineering, model configuration, output parsing, and UI layers.

---

## What I Did

- Designed a `ChatPromptTemplate` with a system message enforcing step-by-step classification (tone → intent → style → summary) before returning structured output
- Defined a `ReviewFormat` Pydantic schema with `Field`-level descriptions to guide the model's output format
- Wired `PydanticOutputParser` into the chain via `.partial()` injection of `format_instructions` — no manual prompt editing required
- Integrated **Gemini 2.5 Flash Lite** via `langchain_google_genai` for fast, cost-efficient inference
- Built an interactive Streamlit UI with session state to persist results across rerenders without re-invoking the chain
- Kept all modules decoupled: `model.py`, `parser.py`, `prompt_tmp.py`, `main.py` — each with a single responsibility

---

## Features

- Structured output with 4 analysis dimensions: Tone, Intent, Communication Style, Summary
- Pydantic schema validation — malformed LLM outputs are caught at parse time
- LCEL pipeline: composable, readable, easy to swap components
- Session state management in Streamlit — no redundant API calls on UI rerender
- Clean multi-file architecture

---

## Install & Run

```bash
# 1. Clone the repo
git clone https://github.com/<your-username>/social-media-post-analyzer.git
cd social-media-post-analyzer

# 2. Create and activate a virtual environment
python -m venv venv
source venv/bin/activate  # Windows: venv\Scripts\activate

# 3. Install dependencies
pip install -r requirements.txt
pip install streamlit langchain langchain-google-genai

# 4. Set your API key
echo "GOOGLE_API_KEY=your_key_here" > .env

# 5. Run the app
streamlit run main.py
```

---

## Usage

1. Open the app in your browser (default: `http://localhost:8501`)
2. Paste any social media post into the text area
3. Click **Analyze**
4. View the structured breakdown: Tone, Intent, Communication Style, and Summary

**Example input:**
> "Just launched my new course on prompt engineering! 🚀 After 6 months of work, it's finally live. Grab the early-bird discount before it's gone!"

**Example output:**

| Field | Value |
|---|---|
| Tone | Excited, Optimistic |
| Intent | Promotion |
| Communication Style | Announcement, Persuasive |
| Summary | A creator enthusiastically announces the launch of a prompt engineering course with a time-limited discount offer. |

---

## Architecture / Tech Stack

```
User Input (Streamlit UI)
        │
        ▼
ChatPromptTemplate  ──►  Gemini 2.5 Flash Lite (LangChain)  ──►  PydanticOutputParser
        │                                                                  │
        └──────────────────── ReviewFormat (Pydantic) ◄───────────────────┘
                                        │
                              Streamlit Session State
                                        │
                              Rendered UI (st.write)
```

| Layer | Technology |
|---|---|
| LLM | Google Gemini 2.5 Flash Lite |
| Orchestration | LangChain LCEL |
| Output Parsing | PydanticOutputParser |
| UI | Streamlit |
| Config | python-dotenv |

---

## Future Work

- Add confidence scores or alternative classifications per field
- Support batch analysis (CSV upload)
- Add platform-specific context (Twitter vs LinkedIn vs Instagram prompting)
- Export results as JSON / CSV
- Deploy to Streamlit Cloud or Hugging Face Spaces

---


| Signal | Where to look |
|---|---|
| **Prompt engineering** | `prompt_tmp.py` — system message with classification guidelines + format injection |
| **Structured output design** | `parser.py` — Pydantic schema with `Field` descriptions |
| **Clean architecture** | 4 single-responsibility modules, zero circular imports |
| **LangChain LCEL fluency** | `main.py` — composable chain in one line |
| **UI/UX awareness** | Session state prevents redundant API calls; spinner for async feedback |
| **Engineering tradeoffs** | Gemini Flash Lite chosen for speed + cost over heavier models |

---

## Contact

- **GitHub:** [github.com/your-username](https://github.com/Ganeshpithani)
- **LinkedIn:** [linkedin.com/in/your-profile](https://www.linkedin.com/in/ganesh-pithani-a91343250/)
- **Email:** ganeshpithani001@example.com
