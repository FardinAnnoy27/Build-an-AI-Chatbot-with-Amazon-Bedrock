```markdown
# 🤖 Build an AI Chatbot with Amazon Bedrock & Amazon Nova

## 🏗️ Project Overview

Build a Python-powered, multi-turn AI chatbot running within **AWS CloudShell**. The application communicates directly with Amazon Bedrock's next-generation **Amazon Nova Lite** foundation model using the unified Converse API. By the project's conclusion, your chatbot will maintain contextual conversation history, exhibit a customized persona, and enforce enterprise-grade safety configurations.

* **Difficulty:** Easy / Beginner-friendly
* **Time to Complete:** ~60 minutes
* **Estimated Cost:** < $0.01 (Well within AWS Free Tier limits)

---

## 🏛️ System Architecture

The workflow bypasses local dependency installation by utilizing browser-based infrastructure to send API calls directly to AWS managed model runtimes:

```text
       YOU (User)
           │
           │ Type a message
           ▼
     AWS CloudShell (Browser-based execution environment)
           │
           │ Runs python script: `bedrock_chat.py`
           │ Leverages `boto3` (AWS SDK for Python)
           ▼
     Amazon Bedrock (Unified Converse API Layer)
           │
           │ Forwards payloads & configurations
           ▼
     Amazon Nova Lite (`amazon.nova-lite-v1:0`)
           │
           │ Generates contextually grounded completion
           ▼
Response Stream ➔ Python Parser ➔ Terminal Display

```

### Core Components Matrix

| Component | Architecture Role | Key Technical Benefit |
| --- | --- | --- |
| **AWS CloudShell** | Execution Environment | Free, browser-based Linux terminal. Eliminates local Python / credential configuration. |
| **boto3** | Application SDK | Official AWS SDK for Python used to authenticate and execute API sessions. |
| **Amazon Bedrock** | Serverless AI Gateway | Fully managed orchestration service providing instant serverless access to foundation models. |
| **Converse API** | High-level API Structure | Bedrock's unified interface for managing multi-turn states, system prompts, and guardrails. |
| **Amazon Nova Lite** | Core Foundation Model | ID: `amazon.nova-lite-v1:0`. Efficient, low-latency model optimized for conversational AI. |

---

## 📋 Step-by-Step Implementation Pipeline

### Step 0: Context Initialization

* **Objective:** Establish scope, define project outcomes, and explicitly track baseline parameters.

### Step 1: Console Exploration & Model Activation

* **Objective:** Gain hands-on familiarity with the AWS ecosystem before initializing code.

1. Authenticate into your **AWS Management Console**.
2. Transition your deployment region to **`us-east-1` (N. Virginia)**.
3. Navigate to **Amazon Bedrock** ➔ access **Model Access** to confirm your account has permissions for the Amazon Nova series.
4. Launch the native **Chat Playground**, select **Amazon Nova Lite**, and execute test prompts directly through the UI.

> **Key Takeaway:** Understanding the console ecosystem allows you to visually audit runtime parameters before writing their programmatic equivalents.

### Step 2: Formulating the First API Request

* **Objective:** Transition from visual components to automated programmatic execution.

1. Spin up **AWS CloudShell** from the top console toolbar.
2. Verify environment baselines by validating native dependencies:

```powershell
   python3 --version
   pip show boto3

```

3. Initialize an isolated file named `bedrock_chat.py` and invoke the endpoint using the high-level framework:

```python
   # High-level conceptual baseline
   response = client.converse(
       modelId="amazon.nova-lite-v1:0",
       messages=[{"role": "user", "content": [{"text": "Hello Nova!"}]}]
   )

```

4. Run your script via terminal and intercept your first serverless model compilation.

### Step 3: Engineering Contextual Memory & Personas

* **Objective:** Expand the static script into an active, multi-turn conversational loop.

1. **Inject System Context:** Persona Tuning.
Pass a system prompt argument inside the Converse API structure to permanently mold the AI's persona (e.g., instructing it to respond as a concise systems engineer).


2. **Build an Appending Message Array:** State Management.
Initialize a persistent Python list containing message dictionaries. Append both incoming user inputs and outgoing assistant responses to maintain history across turns.


3. **Create a Continuous Loop:** Runtime Stream.
Wrap the script execution flow inside a `while True:` loop, giving users an interactive prompt breakable via `exit`.


4. **Optimize Hyperparameters:** Inference Tuning.
Expose and alter inference configurations (`temperature` to control stochastic randomness, `maxTokens` to constrain output caps).


---

## 💎 Secret Mission: Responsible AI Guardrails

**Objective:** Secure your application layer from adversarial exploits using structural firewalls.

* Navigate to Bedrock's **Guardrails** dashboard and provision a custom moderation policy.
* Define strict toxicity matrices, structural content filtering levels, and phrase exclusions.
* Extract your unique `guardrailIdentifier` and `guardrailVersion`, appending them cleanly to your existing client connection code within `bedrock_chat.py`.
* Force-test security policies with sensitive prompts to observe automated interception behaviors.

---
