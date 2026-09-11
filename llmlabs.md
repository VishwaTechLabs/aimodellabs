# 🧪 LLM Temperature Testing Lab

[![Python](https://img.shields.io/badge/Python-3.x-blue?logo=python&logoColor=white)](https://www.python.org/)
[![LLM Testing](https://img.shields.io/badge/Testing-LLM%20Testing-purple)](#-what-is-llm-testing)
[![AI Quality](https://img.shields.io/badge/AI-Quality%20Engineering-orange)](#-what-are-we-building-toward)
[![API](https://img.shields.io/badge/API-LLM%20API-green)](#-what-is-an-llm-api)
[![Experiment](https://img.shields.io/badge/Lab-Experiment-red)](#-experiment-2--temperature-comparison)
[![License](https://img.shields.io/badge/License-Educational-lightgrey)](#)

> 🎯 **VishwaTech-Labs — AI Testing / LLM Quality Engineering**

---

## 📚 Table of Contents

- [🎯 Lab Objective](#-lab-objective)
- [🧠 What Are We Actually Testing?](#-what-are-we-actually-testing)
- [🤖 What Is an LLM?](#-what-is-an-llm)
- [🔌 What Is an LLM API?](#-what-is-an-llm-api)
- [🐍 Why Use Python?](#-why-use-python)
- [🌡️ What Is Temperature?](#️-what-is-temperature)
- [🎯 Why Should a Tester Care About Temperature?](#-why-should-a-tester-care-about-temperature)
- [🏗️ Lab Architecture](#️-lab-architecture)
- [🧰 Prerequisites](#-prerequisites)
- [🚀 Lab Setup](#-lab-setup)
- [🧪 Experiment 1 — Basic LLM API Call](#-experiment-1--basic-llm-api-call)
- [🌡️ Experiment 2 — Temperature Comparison](#️-experiment-2--temperature-comparison)
- [🔁 Experiment 3 — Repeatability Testing](#-experiment-3--repeatability-testing)
- [📊 Experiment 4 — Automated Evaluation](#-experiment-4--automated-evaluation)
- [🧑‍💻 Complete Test Script](#-complete-test-script)
- [🧪 Test Case Design](#-test-case-design)
- [📋 Test Data Matrix](#-test-data-matrix)
- [🔍 What Should the Tester Observe?](#-what-should-the-tester-observe)
- [📏 LLM Evaluation](#-llm-evaluation)
- [⚖️ Traditional Testing vs LLM Testing](#️-traditional-testing-vs-llm-testing)
- [🚨 Important Testing Rules](#-important-testing-rules)
- [📍 Where Is This Used?](#-where-is-this-used)
- [🏆 What Are We Building Toward?](#-what-are-we-building-toward)
- [📖 Important Definitions](#-important-definitions)
- [✅ Lab Checklist](#-lab-checklist)
- [🐛 Troubleshooting](#-troubleshooting)
- [🎓 Final Learning Outcome](#-final-learning-outcome)

---

# 🎯 Lab Objective

The objective of this lab is to understand how an application can communicate with a **Large Language Model (LLM)** through an API and how a tester can perform a controlled experiment by changing the **temperature** parameter.

We will progress through:

```text
Python
   ↓
LLM API
   ↓
LLM Model
   ↓
Generated Response
   ↓
Capture Response
   ↓
Change Temperature
   ↓
Compare Responses
   ↓
Evaluate Quality
   ↓
Automate Testing
```

By the end of this lab, you should be able to say:

> 💡 **"I can programmatically call an LLM, control supported generation parameters, capture multiple responses, compare them, and evaluate their quality from a testing perspective."**

---

# 🧠 What Are We Actually Testing?

Traditional software testing normally follows:

```text
Input
  ↓
Application
  ↓
Expected Output
  ↓
Actual Output
  ↓
PASS / FAIL
```

Example:

```text
Input:
2 + 3

Expected:
5

Actual:
5

Result:
PASS
```

With an LLM, things are different.

We may have:

```text
Prompt
  ↓
LLM
  ↓
Generated Response
```

For the same prompt, several different responses may be valid.

Example:

### Prompt

```text
Explain Kubernetes in simple words.
```

### Response A

```text
Kubernetes is a platform used to manage containers.
```

### Response B

```text
Kubernetes helps organizations deploy, scale, and manage
containerized applications.
```

### Response C

```text
Kubernetes is like a manager for containerized workloads.
```

All three could potentially be acceptable.

Therefore, AI testing often requires more than exact string comparison.

---

# 🤖 What Is an LLM?

## Definition

**LLM = Large Language Model**

An LLM is a machine-learning model trained on large amounts of data to understand and generate language.

At a high level:

```text
Prompt
   ↓
LLM
   ↓
Token generation
   ↓
Response
```

Examples of tasks:

- Answering questions
- Summarizing text
- Generating code
- Translating
- Classification
- Extraction
- Reasoning
- Content generation
- Conversational interaction

---

# 🔌 What Is an LLM API?

An **API (Application Programming Interface)** allows your application to communicate with an LLM service.

Instead of manually opening a chatbot and typing:

```text
Explain Kubernetes.
```

your Python program can send the request.

Conceptually:

```text
Python Application
       |
       | API Request
       ↓
   LLM Service
       |
       ↓
      Model
       |
       | Generated Response
       ↓
Python Application
```

For current OpenAI API information, models, authentication, and supported parameters, refer to the [official OpenAI API documentation](https://platform.openai.com/docs/overview).

---

# 🐍 Why Use Python?

We could manually perform the experiment:

```text
Open application
     ↓
Enter prompt
     ↓
Change temperature
     ↓
Copy response
     ↓
Repeat 15 times
     ↓
Compare manually
```

That is not scalable.

Python allows us to automate:

```text
Generate prompt
     ↓
Call API
     ↓
Capture response
     ↓
Repeat
     ↓
Store results
     ↓
Evaluate
     ↓
Generate report
```

This is where normal testing starts becoming:

> 🚀 **AI Quality Engineering**

---

# 🌡️ What Is Temperature?

## Definition

> **Temperature is a model-generation parameter that influences the amount of variation in generated responses.**

A useful mental model:

```text
                 TEMPERATURE
                      │
          ┌───────────┴───────────┐
          ↓                       ↓
       LOWER                    HIGHER
          ↓                       ↓
   More predictable          More variation
   More focused              Potentially creative
   Less variation             Less predictable
```

However:

> ⚠️ Temperature does **not** make the model more intelligent.

It influences **generation behavior**.

---

# 🧠 Temperature — Tester Perspective

Suppose we have:

```text
Prompt:
Write a description of Kubernetes.
```

We test:

```text
Temperature = 0.2
Temperature = 0.7
Temperature = 1.0
```

We are asking:

> ❓ Does changing the temperature influence the generated response?

This is an **experiment**.

We are changing one variable while attempting to keep the other important variables controlled.

---

# 🎯 Why Should a Tester Care About Temperature?

Imagine a customer-support chatbot.

User:

```text
What is your refund policy?
```

We want:

```text
Correct
Consistent
Relevant
Grounded
Safe
```

We don't want the chatbot to randomly invent different policies.

For creative applications, however, variation can be useful.

Examples:

### Customer Support

```text
Consistency ↑
Correctness ↑
Hallucination ↓
```

### Marketing Content

```text
Variation ↑
Creativity ↑
```

### Code Generation

```text
Correctness ↑
Security ↑
Instruction Following ↑
```

Therefore, testers need to understand how generation settings can influence application behavior.

---

# 🏗️ Lab Architecture

Our basic architecture:

```text
┌─────────────────────────┐
│     Python Test         │
│        Script           │
└────────────┬────────────┘
             │
             │ API Request
             ↓
┌─────────────────────────┐
│       LLM API           │
└────────────┬────────────┘
             │
             ↓
┌─────────────────────────┐
│      LLM Model          │
└────────────┬────────────┘
             │
             │ Response
             ↓
┌─────────────────────────┐
│   Python Response       │
│       Capture           │
└────────────┬────────────┘
             │
             ↓
┌─────────────────────────┐
│       Evaluation        │
└─────────────────────────┘
```

---

# 🧰 Prerequisites

You need:

| Requirement | Purpose |
|---|---|
| 🐍 Python 3.x | Run the test program |
| 🌐 Internet | Communicate with API |
| 🔑 API access | Authenticate requests |
| 🧑‍💻 IDE/Text Editor | Write Python |
| 📦 OpenAI Python SDK | API communication |

Install Python from:

[Python Official Website](https://www.python.org/)

---

# 🚀 Lab Setup

## Step 1 — Create Lab Directory

```bash
mkdir llm-temperature-lab
cd llm-temperature-lab
```

---

## Step 2 — Create Virtual Environment

```bash
python -m venv venv
```

### Windows PowerShell

```powershell
venv\Scripts\activate
```

### Linux/macOS

```bash
source venv/bin/activate
```

You should see something similar to:

```text
(venv)
```

in your terminal prompt.

---

# 📦 Step 3 — Install OpenAI SDK

```bash
pip install openai
```

Verify:

```bash
pip show openai
```

You should see package information.

---

# 🔐 Step 4 — Configure API Key

Never hard-code your real API key in Python.

### ❌ Bad Practice

```python
api_key = "sk-xxxxxxxxxxxxxxxx"
```

Do not commit API keys to GitHub.

---

## ✅ Windows PowerShell

```powershell
$env:OPENAI_API_KEY="YOUR_API_KEY"
```

---

## ✅ Linux/macOS

```bash
export OPENAI_API_KEY="YOUR_API_KEY"
```

The Python SDK can then obtain the key from the environment.

---

# 🧪 Experiment 1 — Basic LLM API Call

## 🎯 Objective

Verify that Python can successfully communicate with an LLM API.

---

## Create File

```text
test_llm.py
```

---

## Python Code

```python
from openai import OpenAI

client = OpenAI()

response = client.responses.create(
    model="YOUR_MODEL",
    input="Explain Kubernetes in simple words."
)

print(response.output_text)
```

Replace:

```text
YOUR_MODEL
```

with a model available to your API account.

---

## ▶️ Execute

```bash
python test_llm.py
```

---

## Expected Result

You should receive a generated response similar to:

```text
Kubernetes is a platform used to deploy,
manage, and scale containerized applications.
```

The exact wording will vary.

---

# 🔎 What Just Happened?

Your Python program acted as the API client.

```text
Python
  │
  │ Request
  ↓
LLM API
  │
  ↓
Model
  │
  │ Response
  ↓
Python
```

Important:

> 🧠 Your Python script is calling the hosted model through the API. It is not automatically executing the LLM locally.

---

# 🌡️ Experiment 2 — Temperature Comparison

## 🎯 Objective

Determine whether changing temperature changes generated response behavior.

---

## Controlled Variables

Keep these constant:

```text
Prompt
Model
System instructions
Input data
Relevant API settings
```

Change only:

```text
Temperature
```

Example:

```text
Temperature 0.2
Temperature 0.7
Temperature 1.0
```

---

# ⚠️ Important API Note

Not every model/API combination necessarily supports every parameter.

Therefore:

> **Before running a temperature experiment, verify that the selected model and API endpoint support `temperature`.**

Do not assume that every current LLM model accepts the parameter in exactly the same way.

---

# 🧑‍💻 Temperature Experiment Code

For an API/model that supports the Chat Completions API and temperature:

```python
from openai import OpenAI

client = OpenAI()

prompt = "Write a short description of Kubernetes."

temperatures = [0.2, 0.7, 1.0]

for temperature in temperatures:

    response = client.chat.completions.create(
        model="YOUR_MODEL",
        messages=[
            {
                "role": "user",
                "content": prompt
            }
        ],
        temperature=temperature
    )

    print("=" * 60)
    print(f"Temperature: {temperature}")
    print("=" * 60)
    print(response.choices[0].message.content)
```

Run:

```bash
python test_temperature.py
```

---

# 📊 Expected Experiment Structure

You may observe:

```text
============================================================
Temperature: 0.2
============================================================

Response A


============================================================
Temperature: 0.7
============================================================

Response B


============================================================
Temperature: 1.0
============================================================

Response C
```

The responses may differ in:

- wording
- sentence structure
- examples
- ordering
- phrasing
- token selection
- overall variation

---

# 🔬 What Are We Actually Measuring?

We are not simply asking:

> "Are the answers different?"

We should ask:

### 1️⃣ Variability

How much do responses differ?

```text
Response A
Response B
Response C
Response D
```

---

### 2️⃣ Consistency

Do repeated requests produce acceptably similar behavior?

```text
Run 1 → A
Run 2 → A
Run 3 → A
Run 4 → B
Run 5 → A
```

---

### 3️⃣ Correctness

Are the generated answers factually correct?

---

### 4️⃣ Relevance

Does the answer remain related to the prompt?

---

### 5️⃣ Instruction Following

Did the model follow the requested instructions?

---

### 6️⃣ Safety

Does the response remain within the required safety boundaries?

---

### 7️⃣ Groundedness

If the application supplies source information, is the response supported by that information?

---

# 🔁 Experiment 3 — Repeatability Testing

Testing only one response per temperature is weak.

Instead:

```text
Temperature 0.2
    ├── Run 1
    ├── Run 2
    ├── Run 3
    ├── Run 4
    └── Run 5

Temperature 0.7
    ├── Run 1
    ├── Run 2
    ├── Run 3
    ├── Run 4
    └── Run 5

Temperature 1.0
    ├── Run 1
    ├── Run 2
    ├── Run 3
    ├── Run 4
    └── Run 5
```

This produces:

```text
3 temperatures × 5 runs = 15 responses
```

Now we have much better experimental data.

---

# 🧪 Repeatability Test

Example:

```text
Prompt:
Explain Kubernetes in three sentences.
```

Run:

```text
Temperature = 0.2
5 times
```

Then:

```text
Temperature = 0.7
5 times
```

Then:

```text
Temperature = 1.0
5 times
```

---

# 📊 Example Result Matrix

| Temperature | Run | Response |
|---:|---:|---|
| 0.2 | 1 | A |
| 0.2 | 2 | A |
| 0.2 | 3 | A |
| 0.2 | 4 | B |
| 0.2 | 5 | A |
| 0.7 | 1 | C |
| 0.7 | 2 | D |
| 0.7 | 3 | C |
| 0.7 | 4 | E |
| 0.7 | 5 | D |
| 1.0 | 1 | F |
| 1.0 | 2 | G |
| 1.0 | 3 | H |
| 1.0 | 4 | G |
| 1.0 | 5 | I |

> ⚠️ The above responses are illustrative labels, not actual model outputs.

---

# 📊 Experiment 4 — Automated Evaluation

Now we move from:

```text
"I called an LLM."
```

to:

```text
"I tested an LLM."
```

And eventually:

```text
"I automated LLM quality testing."
```

Our framework becomes:

```text
Test Prompt
     ↓
Test Parameter
     ↓
LLM API
     ↓
Response
     ↓
Evaluation
     ↓
Score
     ↓
PASS / FAIL / REVIEW
```

---

# 🧪 Test Case Design

## TC-LLM-001

### Objective

Verify basic LLM API connectivity.

### Input

```text
Explain Kubernetes in simple words.
```

### Expected Behavior

```text
API returns a response successfully.
```

### Result

```text
PASS / FAIL
```

---

## TC-LLM-002

### Objective

Observe low-temperature response behavior.

### Temperature

```text
0.2
```

### Input

```text
Write a short description of Kubernetes.
```

### Expected Observation

```text
Response should be generated successfully.
Record the actual response for comparison.
```

---

## TC-LLM-003

### Objective

Observe medium-temperature response behavior.

### Temperature

```text
0.7
```

### Input

```text
Write a short description of Kubernetes.
```

---

## TC-LLM-004

### Objective

Observe higher-temperature response behavior.

### Temperature

```text
1.0
```

### Input

```text
Write a short description of Kubernetes.
```

---

## TC-LLM-005

### Objective

Measure repeatability.

### Configuration

```text
Temperature = 0.2
Runs = 5
```

---

## TC-LLM-006

### Objective

Measure higher-temperature variability.

### Configuration

```text
Temperature = 1.0
Runs = 5
```

---

# 📋 Test Data Matrix

| Test ID | Prompt | Temperature | Runs | Purpose |
|---|---|---:|---:|---|
| TC-001 | Explain Kubernetes | N/A | 1 | API connectivity |
| TC-002 | Explain Kubernetes | 0.2 | 1 | Low-temperature behavior |
| TC-003 | Explain Kubernetes | 0.7 | 1 | Medium-temperature behavior |
| TC-004 | Explain Kubernetes | 1.0 | 1 | Higher-temperature behavior |
| TC-005 | Explain Kubernetes | 0.2 | 5 | Repeatability |
| TC-006 | Explain Kubernetes | 0.7 | 5 | Medium variability |
| TC-007 | Explain Kubernetes | 1.0 | 5 | Higher variability |

---

# 🔍 What Should the Tester Observe?

## ✏️ 1. Wording

Does wording change?

```text
Response A:
Kubernetes manages containers.

Response B:
Kubernetes orchestrates containerized applications.
```

---

## 🏗️ 2. Structure

Does the response structure change?

```text
Paragraph
```

versus:

```text
Bullet points
```

versus:

```text
Numbered explanation
```

---

## 🔀 3. Variability

How different are multiple responses?

---

## ✅ 4. Correctness

Are the responses technically correct?

---

## 🎯 5. Relevance

Does the response answer the question?

---

## 📏 6. Instruction Following

Example:

```text
Prompt:
Explain Kubernetes in exactly 3 sentences.
```

Tester checks:

```text
3 sentences?
    ↓
YES → PASS
NO  → FAIL
```

---

## 🛡️ 7. Safety

Does the response violate application safety requirements?

---

# 📏 LLM Evaluation

Traditional testing often uses:

```text
Expected Output == Actual Output
```

For LLMs, this may not be sufficient.

Instead:

```text
                 LLM Response
                      │
          ┌───────────┼───────────┐
          ↓           ↓           ↓
     Correctness   Relevance   Safety
          │           │           │
          └───────────┼───────────┘
                      ↓
                  Evaluation
                      ↓
                 Score / Result
```

---

# 📊 Evaluation Dimensions

| Dimension | Tester Question |
|---|---|
| Correctness | Is the information accurate? |
| Relevance | Does it answer the question? |
| Completeness | Are required points covered? |
| Instruction Following | Did it follow the requested format? |
| Safety | Is the response safe? |
| Groundedness | Is it supported by supplied context? |
| Consistency | Is behavior acceptably stable? |
| Fluency | Is the response understandable? |
| Coherence | Does the response make logical sense? |

---

# 📝 Example Evaluation Record

| Temperature | Run | Correctness | Relevance | Instruction | Overall |
|---:|---:|---:|---:|---:|---|
| 0.2 | 1 | 5/5 | 5/5 | 5/5 | PASS |
| 0.2 | 2 | 5/5 | 5/5 | 5/5 | PASS |
| 0.7 | 1 | 5/5 | 5/5 | 4/5 | REVIEW |
| 1.0 | 1 | 4/5 | 5/5 | 5/5 | REVIEW |

> ⚠️ These scores are examples only. A real project should define its own evaluation rubric.

---

# ⚖️ Traditional Testing vs LLM Testing

| Traditional Software | LLM / AI |
|---|---|
| Exact expected output often available | Multiple valid responses may exist |
| String comparison common | Semantic/quality evaluation often required |
| Deterministic behavior common | Probabilistic behavior can occur |
| PASS/FAIL often straightforward | Scoring/rubrics may be required |
| Functional correctness dominates | Correctness + relevance + safety + grounding |
| Regression tests compare outputs | Regression tests compare quality/behavior |

---

# 🚨 Important Testing Rules

## Rule 1 — Don't change everything at once

Bad experiment:

```text
Change model
Change prompt
Change temperature
Change system instruction
Change input
```

You won't know what caused the difference.

Better:

```text
Model       → SAME
Prompt      → SAME
Input       → SAME
Instructions → SAME

Temperature → CHANGE
```

This is a controlled experiment.

---

# Rule 2 — Don't conclude from one response

Bad:

```text
Temperature 1.0 generated a different answer.

Therefore:
Temperature causes creativity.
```

This conclusion is too strong.

Better:

```text
Run multiple trials and compare response variation.
```

---

# Rule 3 — Don't confuse temperature with intelligence

Incorrect:

```text
Temperature ↑
      ↓
Intelligence ↑
```

Temperature influences generation behavior.

It does not mean:

```text
higher temperature = smarter model
```

---

# Rule 4 — Don't assume every model behaves identically

Different:

- models
- API endpoints
- parameter implementations
- model versions

can behave differently.

Always verify current API/model documentation.

---

# Rule 5 — Evaluate quality, not only difference

Two responses can be different and both be correct.

Two responses can be similar and both be wrong.

Therefore:

```text
Different ≠ Bad

Same ≠ Correct
```

This is a very important AI testing concept.

---

# 📍 Where Is This Used?

## 💬 Customer Support

Test:

```text
Correctness
Consistency
Groundedness
Safety
```

---

## 👨‍💻 Code Generation

Test:

```text
Correctness
Security
Compilation
Instruction following
Test coverage
```

---

## 📢 Marketing Content

Test:

```text
Relevance
Style
Creativity
Brand compliance
Variation
```

---

## 🤖 Chatbots

Test:

```text
Context handling
Relevance
Safety
Consistency
Instruction following
```

---

## 📚 RAG Applications

Test:

```text
Retrieval
Groundedness
Faithfulness
Citation accuracy
Answer correctness
```

---

# 🏆 What Are We Building Toward?

This basic lab is the starting point.

Eventually we can build:

```text
                 AI QUALITY ENGINEERING
                         │
                         ↓
                  Test Data
                         │
                         ↓
                    Test Prompts
                         │
                         ↓
                  Test Parameters
                         │
                         ↓
                     LLM API
                         │
                         ↓
                    Responses
                         │
        ┌────────────────┼────────────────┐
        ↓                ↓                ↓
   Correctness       Relevance        Safety
        ↓                ↓                ↓
   Grounding        Consistency      Compliance
        └────────────────┼────────────────┘
                         ↓
                     Evaluation
                         ↓
                     Scoring
                         ↓
                  Regression Tests
                         ↓
                      Reports
                         ↓
                       CI/CD
                         ↓
                  Quality Gate
```

---

# 🚀 The AI Testing Journey

Your learning progression should look like:

```text
"I know how to test software."
              ↓
"I know how to test AI."
              ↓
"I can call an LLM API."
              ↓
"I can automate LLM tests."
              ↓
"I can evaluate AI quality."
              ↓
"I can measure AI quality."
              ↓
"I can build AI Quality Engineering frameworks."
              ↓
"I can integrate AI quality into CI/CD."
              ↓
"I can prevent poor-quality AI releases."
```

---

# 📖 Important Definitions

| Term | Definition |
|---|---|
| **LLM** | Large Language Model used to process and generate language. |
| **API** | Interface that allows software to communicate with a service. |
| **Prompt** | Input or instructions supplied to the model. |
| **Token** | A unit used by a language model to process/generate text. |
| **Parameter** | A configurable setting that can influence behavior. |
| **Temperature** | Generation parameter that influences response variation. |
| **Test Case** | Defined test scenario containing inputs, conditions, checks and expected behavior. |
| **Test Input** | Prompt/data sent to the model. |
| **Actual Output** | Response generated by the model. |
| **Expected Behavior** | Behavior that the tester expects based on requirements. |
| **Evaluation** | Judging an AI response against defined criteria. |
| **Variability** | Degree to which responses differ between runs. |
| **Consistency** | Degree to which repeated responses remain acceptably similar. |
| **Hallucination** | Unsupported or false information generated by an AI system. |
| **Groundedness** | Degree to which an answer is supported by supplied source/context. |
| **Regression Testing** | Re-running tests after changes to detect quality degradation. |
| **AI Quality Engineering** | Engineering discipline focused on measuring, automating and controlling AI-system quality. |

---

# 🧪 Complete Lab Flow

```text
                    START
                      │
                      ↓
              Install Python
                      │
                      ↓
              Create venv
                      │
                      ↓
             Install OpenAI SDK
                      │
                      ↓
             Configure API Key
                      │
                      ↓
             Make First API Call
                      │
                      ↓
                 API PASS?
                /          \
              NO            YES
              │              │
              ↓              ↓
        Troubleshoot     Test Temperature
                             │
                             ↓
                       0.2 / 0.7 / 1.0
                             │
                             ↓
                        Repeat Runs
                             │
                             ↓
                      Capture Results
                             │
                             ↓
                         Evaluate
                             │
                             ↓
                      Compare Results
                             │
                             ↓
                      Generate Report
                             │
                             ↓
                         END
```

---

# 🧑‍💻 Recommended Project Structure

```text
llm-temperature-lab/
│
├── README.md
│
├── venv/
│
├── test_llm.py
│
├── test_temperature.py
│
├── test_repeatability.py
│
├── results/
│   └── temperature_results.csv
│
└── reports/
    └── llm_temperature_report.md
```

---

# 🧪 Suggested Future Labs

After completing this temperature lab, continue with:

```text
LAB 01
Basic LLM API Testing
        ↓
LAB 02
Temperature Testing
        ↓
LAB 03
Prompt Testing
        ↓
LAB 04
Prompt Injection Testing
        ↓
LAB 05
Hallucination Testing
        ↓
LAB 06
Groundedness Testing
        ↓
LAB 07
RAG Testing
        ↓
LAB 08
LLM Safety Testing
        ↓
LAB 09
LLM Evaluation
        ↓
LAB 10
LLM Regression Testing
        ↓
LAB 11
Automated LLM Testing
        ↓
LAB 12
CI/CD AI Quality Gates
```

---

# 🐛 Troubleshooting

## ❌ `ModuleNotFoundError: No module named 'openai'`

Run:

```bash
pip install openai
```

Verify:

```bash
pip show openai
```

---

## ❌ API Key Error

Check that the environment variable exists.

### Windows PowerShell

```powershell
$env:OPENAI_API_KEY
```

### Linux/macOS

```bash
echo $OPENAI_API_KEY
```

Do not print or expose your actual API key in logs, screenshots, Git repositories, or reports.

---

## ❌ Authentication Error

Check:

```text
API key
Account/API access
Environment variable
Selected provider
```

---

## ❌ Model Error

If you receive a model-related error:

```text
YOUR_MODEL
```

may not be available to your API account.

Use a currently supported model listed in the provider's official documentation.

---

## ❌ Temperature Parameter Error

If the API rejects:

```python
temperature=...
```

check whether the selected model and API endpoint support the parameter.

Do not assume all models expose identical parameters.

---

# ✅ Lab Checklist

Before declaring the lab complete:

- [ ] Python installed
- [ ] Virtual environment created
- [ ] Virtual environment activated
- [ ] LLM SDK installed
- [ ] API access configured
- [ ] API key stored securely
- [ ] First API call successful
- [ ] Model availability verified
- [ ] Temperature support verified
- [ ] Same prompt used for comparison
- [ ] Temperature changed systematically
- [ ] Multiple runs performed
- [ ] Responses captured
- [ ] Responses compared
- [ ] Correctness evaluated
- [ ] Relevance evaluated
- [ ] Instruction following evaluated
- [ ] Safety considered
- [ ] Results documented
- [ ] Conclusions based on observed data

---

# 🎓 Final Learning Outcome

At the beginning of this lab:

> ❌ "I only know how to use an AI chatbot."

After Experiment 1:

> 🟢 "I can call an LLM using Python."

After Experiment 2:

> 🟢 "I can change an LLM generation parameter."

After Experiment 3:

> 🟢 "I can perform repeated LLM experiments."

After Experiment 4:

> 🟢 "I can evaluate LLM responses."

And eventually:

> 🚀 **"I can build automated AI Quality Engineering frameworks and integrate AI quality checks into CI/CD."**

---

# 🧠 One-Sentence Definition for Students

> **Temperature testing is a controlled LLM experiment in which a tester changes the supported generation temperature while keeping other important conditions controlled, then compares multiple generated responses to understand variability and evaluate response quality.**

---

# ⭐ The Most Important Tester Lesson

Remember this:

```text
Traditional Testing
        ↓
"Is the output exactly what I expected?"

LLM Testing
        ↓
"Is the response correct?"
        +
"Is it relevant?"
        +
"Did it follow instructions?"
        +
"Is it safe?"
        +
"Is it grounded?"
        +
"Is the variability acceptable?"
        +
"Did the model quality regress?"
```

That mindset is the foundation of **LLM Testing and AI Quality Engineering**.

---

## 🔗 Official Resources

- [Python](https://www.python.org/)
- [OpenAI API Documentation](https://platform.openai.com/docs/overview)
- [OpenAI API Reference](https://platform.openai.com/docs/api-reference)

---

# 🏁 END OF LAB

**VishwaTech-Labs**

### AI Testing • LLM Testing • AI Quality Engineering • DevSecOps • Cloud Security
