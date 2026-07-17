# 🤖 Azure LangGraph E2E Test Agent

An AI-powered end-to-end testing agent that converts natural-language test requirements into executable asynchronous Playwright tests. 🧪

It uses Azure OpenAI to plan atomic actions, inspect the live page DOM, generate Playwright code, execute the completed test with Pytest, and produce a clear test report.

## ✨ Features

- 📝 Converts business-level test scenarios into atomic browser actions
- 🧠 Uses Azure OpenAI for test planning and Playwright code generation
- 🔀 Orchestrates the workflow with LangGraph
- 🌐 Captures current DOM state before generating each action
- 🎯 Prefers `data-testid` selectors when available
- ✅ Validates generated Python syntax before adding it to a test
- ▶️ Runs generated asynchronous tests with Pytest and Playwright
- 📊 Produces reports with actions, results, and generated test code

## 🏗️ How it works

```text
Natural-language test request
          ↓
🧠 Azure OpenAI creates atomic test actions
          ↓
🔀 LangGraph orchestrates each action
          ↓
🌐 Playwright opens the target site and captures DOM state
          ↓
💻 Azure OpenAI generates the next Playwright action
          ↓
✅ Generated code is validated and added to the test
          ↓
🧪 Pytest executes the completed test
          ↓
📊 The agent produces a test-generation report
```

## 🛠️ Tech stack

- 🐍 Python
- ☁️ Azure OpenAI (`AzureChatOpenAI`)
- ⛓️ LangChain
- 🔀 LangGraph
- 🎭 Playwright
- 🧪 Pytest / pytest-playwright
- 🌶️ Flask
- 📓 Jupyter / IPython

## 📋 Prerequisites

- Python 3.10+
- An Azure OpenAI resource with a deployed chat model
- Playwright browser binaries
- A web application available at the target URL
- Optional: Flask for the included local sample application

## 🚀 Installation

Clone the repository and create a virtual environment:

```bash
git clone https://github.com/<your-username>/azure-langgraph-e2e-test-agent.git
cd azure-langgraph-e2e-test-agent

python -m venv .venv
source .venv/bin/activate
```

On Windows:

```powershell
.venv\Scripts\Activate.ps1
```

Install dependencies:

```bash
pip install \
  langchain==1.3.13 \
  langchain-community==0.4.2 \
  langchain-core==1.4.9 \
  langchain-experimental==0.4.2 \
  langchain-openai==1.3.5 \
  langchain-text-splitters==1.1.2 \
  langgraph==1.2.9 \
  langgraph-checkpoint==4.1.1 \
  python-dotenv==1.2.2 \
  openai==2.46.0 \
  Flask==3.1.3 \
  pytest-playwright==0.8.0 \
  ipytest==0.14.2 \
  nest-asyncio==1.6.0
```

Install Chromium for Playwright:

```bash
playwright install chromium
```

## ⚙️ Configuration

Create a `.env` file in the project root:

```env
AZURE_OPENAI_API_KEY=your-azure-openai-api-key
AZURE_OPENAI_ENDPOINT=https://your-resource-name.openai.azure.com/
AZURE_OPENAI_API_VERSION=your-api-version

AZURE_OPENAI_LLM_MODEL=your-model-name
AZURE_OPENAI_LLM_MODEL_DEPLOYMENT=your-deployment-name
```

🔒 Never commit `.env` files or API keys. Add these entries to `.gitignore`:

```gitignore
.env
.venv/
__pycache__/
.pytest_cache/
playwright-report/
test-results/
```

## ▶️ Usage

Start the target application. For example, with Flask:

```bash
flask --app data/e2e_testing_agent_app.py run
```

Then submit the test request and application URL:

```python
query = """
Navigate to the registration page using the target URL.
Enter a valid username in the username field.
Enter a valid password in the password field.
Enter the same password in the password confirmation field.
Submit the registration form.
Verify that registration succeeds.
"""

target_url = "http://localhost:5000"

result = await run_workflow(query, target_url)
print(result["report"])
```

## 📊 Example output

```text
# Test Generation Report

Generated one test called test_user_registration for the endpoint
http://localhost:5000.

## Test Evaluation Result
1 passed

## Actions Taken During The Test Case
1. Navigate to the registration page using the target URL.
2. Enter a valid username in the username field.
3. Enter a valid password in the password field.
4. Confirm the password.
5. Submit the registration form.
6. Verify registration succeeds.
```

## 🔄 Workflow

1. 📝 Convert the user request into atomic test actions.
2. 🌐 Initialize Playwright and navigate to the target URL.
3. 🔎 Retrieve the current page DOM.
4. 💻 Generate Playwright code for the current action.
5. ✅ Validate the generated code.
6. 🔁 Repeat until all actions are complete.
7. 🧪 Wrap the script as an asynchronous Pytest test.
8. ▶️ Execute the test and collect Pytest output.
9. 📊 Generate a final test report.

## 🔐 Security notes

The workflow instructs the model to treat DOM content as untrusted input. Because generated code is dynamically executed, use this project only in controlled development or test environments.

For production use, consider selector allowlists, code sandboxing, human approval before execution, test isolation, retries, screenshots or videos on failure, structured logging, and persistent LangGraph checkpoints.

## 📄 License

MIT License
