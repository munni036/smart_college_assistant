# AI-Powered Smart College Assistant

A robust, conversational AI assistant for college management powered by **LangChain's Tool Calling Agent** (`create_tool_calling_agent` + `AgentExecutor`). It allows students to query attendance, check examination eligibility, calculate grades, view fee balances, compute library fines, calculate hostel fee structures, and look up student details.

---

## 🌟 Features

1. **Attendance & Eligibility Calculator**: Computes attendance percentage and determines exam eligibility (minimum 75% attendance required).
2. **Academic Results & Grades Calculator**: Computes average marks, assigns grades (A, B, C, D), and indicates pass/fail status across 5 subjects.
3. **Fee Balance Tracker**: Calculates outstanding course tuition fee balances.
4. **Library Fine Calculator**: Calculates overdue book fines at a rate of Rs. 5 per delayed day.
5. **Hostel Fee Calculator**: Computes hostel boarding fees based on monthly rates and duration of stay.
6. **Student Directory Information (Bonus)**: Looks up student profile details (Name, Branch, Year) from the database by Student ID.

---

## 🛠️ Key Technical Highlights

* **Small Local LLM Optimization**: Local models like `qwen2.5:0.5b` are prone to parameter swapping or omission. To solve this, custom **Pydantic Pre-Validators** (`@model_validator(mode='before')`) intercept raw LLM outputs, auto-correcting swapped numbers (e.g., ensuring `total_classes >= attended_classes`) and filling default values dynamically.
* **Safe Output Callback Handler**: Standard LangChain stdout callbacks can cause encoding crashes on Windows consoles containing symbol prints (like the Indian Rupee sign `₹`). A custom `SafeVerboseHandler` interceptor handles all logs gracefully in UTF-8 format.
* **Dual LLM Provider Support**: Runs on a local **Ollama** model (defaulting to `qwen2.5:0.5b`) or can switch to **OpenAI** via environment variables.

---

## 🚀 Getting Started

### 1. Prerequisites
- Python 3.10 or higher installed.
- (Optional) [Ollama](https://ollama.com/) running locally with the `qwen2.5:0.5b` model pulled:
  ```bash
  ollama run qwen2.5:0.5b
  ```

### 2. Installation & Setup
1. Clone the repository to your local machine.
2. Create and activate a Python virtual environment:
   ```bash
   # Windows PowerShell
   python -m venv venv
   .\venv\Scripts\activate

   # macOS/Linux
   python3 -m venv venv
   source venv/bin/activate
   ```
3. Install the required dependencies:
   ```bash
   pip install -r requirements.txt
   ```

### 3. Environment Variables (Optional)
By default, the assistant communicates with **Ollama** (`qwen2.5:0.5b`). If you want to switch to **OpenAI**:
```bash
# Windows PowerShell
$env:LLM_PROVIDER="openai"
$env:OPENAI_API_KEY="your-api-key-here"

# macOS/Linux
export LLM_PROVIDER="openai"
export OPENAI_API_KEY="your-api-key-here"
```

---

## 💻 Running the Application

### Option A: Automated Test Suite (Default)
Run a set of 6 pre-configured test queries covering all calculators, single-tool execution, and sequential multi-tool execution:
```bash
python main.py
```

### Option B: Interactive Chat Console
Run the assistant in an interactive CLI loop:
```bash
python main.py --interactive
```

---

## 📂 Project Structure
```text
college_assistant/
├── main.py            # Main application source containing schemas, tools, callback handler, and loop
├── requirements.txt   # Python dependency list
├── .gitignore         # virtual environment
├── README.md          # Project documentation
└── demo.png/          # Contains screenshot demonstrations
```
