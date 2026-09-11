# 🧩 LLM Core Concepts — Tokens, Context, Parameters, Prompts, Sampling & Hallucination

[![LLM](https://img.shields.io/badge/AI-LLM-blue?logo=openai&logoColor=white)](#)
[![Prompt Engineering](https://img.shields.io/badge/Prompt-Prompt%20Engineering-purple)](#)
[![AI Testing](https://img.shields.io/badge/Testing-AI%20Testing-red)](#)
[![Evaluation](https://img.shields.io/badge/Quality-LLM%20Evaluation-green)](#)
[![Python](https://img.shields.io/badge/Python-3.x-blue?logo=python&logoColor=white)](https://www.python.org/)

> 🎓 **VishwaTech-Labs — Understanding How an LLM Request Travels from Prompt → Tokens → Model → Response**

This README explains the operational concepts that every LLM developer, tester, DevSecOps engineer, and AI Quality Engineer should understand.

Topics:

- 🔢 Tokens
- ✂️ Tokenization
- 🪟 Context Window
- ⚙️ Parameters
- 🚀 Inference
- 🌡️ Temperature
- 🎯 Top-P
- 🧑‍💼 System Prompt
- 👤 User Prompt
- 🤖 Assistant Response
- 👻 Hallucination
- 🎲 Model Non-Determinism

---

## 📚 Table of Contents

- [🎯 Objective](#-objective)
- [🧭 End-to-End Request Flow](#-end-to-end-request-flow)
- [1️⃣ Tokens](#1️⃣-tokens)
- [2️⃣ Tokenization](#2️⃣-tokenization)
- [3️⃣ Context Window](#3️⃣-context-window)
- [4️⃣ Parameters](#4️⃣-parameters)
- [5️⃣ Inference](#5️⃣-inference)
- [6️⃣ Temperature](#6️⃣-temperature)
- [7️⃣ Top-P](#7️⃣-top-p)
- [8️⃣ System Prompt](#8️⃣-system-prompt)
- [9️⃣ User Prompt](#9️⃣-user-prompt)
- [🔟 Assistant Response](#-assistant-response)
- [1️⃣1️⃣ Hallucination](#1️⃣1️⃣-hallucination)
- [1️⃣2️⃣ Model Non-Determinism](#1️⃣2️⃣-model-non-determinism)
- [🔗 How Everything Communicates](#-how-everything-communicates)
- [🧪 Complete Tester Workflow](#-complete-tester-workflow)
- [📊 Important Metrics](#-important-metrics)
- [🧪 Test Case Examples](#-test-case-examples)
- [🔐 Security Testing](#-security-testing)
- [🐛 Common Problems](#-common-problems)
- [📖 Key Definitions](#-key-definitions)
- [🚀 Learning Path](#-learning-path)

---

# 🎯 Objective

By the end of this README, you should understand:

```text
Prompt
  ↓
Tokenization
  ↓
Tokens
  ↓
Context
  ↓
Model
  ↓
Inference
  ↓
Logits / probabilities
  ↓
Sampling / decoding
  ↓
Next token
  ↓
Repeat
  ↓
Assistant response
```

You should also understand why the same prompt can sometimes produce different responses.

---

# 🧭 End-to-End Request Flow

Consider:

```text
User:
"Explain Kubernetes in simple words."
```

A simplified flow is:

```text
                 👤 USER
                    │
                    ▼
               User Prompt
                    │
                    ▼
              Tokenization
                    │
                    ▼
                 Tokens
                    │
                    ▼
          Context Construction
                    │
         ┌──────────┼──────────┐
         ▼          ▼          ▼
      System       User      History
      Prompt      Prompt     / Tools
         └──────────┼──────────┘
                    ▼
                 Context
                    │
                    ▼
              LLM Inference
                    │
                    ▼
             Output Logits
                    │
                    ▼
          Decoding / Sampling
                    │
                    ▼
              Next Token
                    │
                    ▼
             Repeat Generation
                    │
                    ▼
            Assistant Response
```

This is simplified. Real serving stacks can include caching, batching, routing, safety systems, tool calls, retrieval, and post-processing.

---

# 1️⃣ Tokens

## 🔢 What Is a Token?

A token is a unit of text representation used by a language model.

A token is **not necessarily one word**.

Depending on the tokenizer, a token can correspond to:

```text
A whole word
Part of a word
Punctuation
Whitespace-related text
Numbers
Special symbols
```

### 🧒 Layman Example

Think of a sentence being cut into puzzle pieces.

```text
"Cloud security is important"
```

becomes something conceptually like:

```text
[Cloud] [ security] [ is] [ important]
```

The exact token boundaries depend on the tokenizer.

### 💻 Technical View

```text
Text
 ↓
Tokenizer
 ↓
Token IDs
 ↓
Model
```

For example:

```text
Text
"Hello world"

       ↓

Token IDs
[... , ...]
```

The actual IDs depend on the tokenizer/vocabulary.

---

# 2️⃣ Tokenization

## ✂️ What Is Tokenization?

Tokenization is the process of converting text into tokens and usually mapping those tokens to integer IDs that the model can process.

```text
Raw Text
   ↓
Tokenizer
   ↓
Tokens
   ↓
Token IDs
```

### 🧒 Layman Example

Imagine a barcode scanner.

```text
Sentence
 ↓
Scanner
 ↓
Pieces / IDs
 ↓
Machine processing
```

### 💻 Technical Pipeline

```text
"Explain cloud security"
            │
            ▼
         Tokenizer
            │
            ▼
     [token₁, token₂, ...]
            │
            ▼
       [id₁, id₂, ...]
            │
            ▼
         Embeddings
            │
            ▼
        Transformer
```

### 🎯 Why Tokenization Matters

Tokenization affects:

- Context usage
- Cost
- Latency
- Maximum input size
- Output size
- Model behavior

Therefore an AI tester should not assume:

```text
1 word = 1 token
```

That is generally false.

---

# 3️⃣ Context Window

## 🪟 What Is a Context Window?

The context window is the amount of tokenized information a model/API can consider within a request according to that model's supported limits.

It may include:

```text
System instructions
User messages
Conversation history
Tool results
Retrieved documents
Other input content
Generated output
```

The exact accounting depends on the model/API.

### 🧒 Layman Example

Imagine a person's working desk.

The desk can only hold so many papers.

```text
┌──────────────────────────────┐
│       CONTEXT WINDOW         │
│                              │
│ System instructions          │
│ Conversation history         │
│ User question                │
│ Retrieved information        │
│ Tool results                 │
│ Output budget / generation   │
└──────────────────────────────┘
```

If the application tries to supply more content than supported, the request may fail, be truncated, or require application-side context management depending on the system.

### 🏗️ RAG Example

```text
User Question
     +
Retrieved Documents
     +
Conversation History
     +
System Instructions
     ↓
Context
     ↓
LLM
     ↓
Answer
```

### ⚠️ Testing Importance

Test:

- Near-limit prompts
- Long conversations
- Large retrieved documents
- Large tool outputs
- Context truncation behavior
- Important instruction placement
- Latency and cost

---

# 4️⃣ Parameters

## ⚙️ What Is a Parameter?

The word **parameter** has two important meanings in AI discussions.

### Meaning A — Model Parameters

These are learned values inside the model.

Examples:

```text
Weights
Biases
```

A large neural model can contain millions, billions, or more learned parameters.

Conceptually:

```text
Training
   ↓
Learn parameters
   ↓
Trained model
```

### Meaning B — API / Generation Parameters

These are settings supplied when making a request.

Examples can include:

```text
Temperature
Top-P
Maximum output limits
Stop conditions
Tool configuration
```

The exact parameters vary by provider and model.

> ⚠️ Do not confuse learned model parameters with API generation settings.

---

# 🧠 Model Parameters vs API Parameters

| Type | Example | Learned? |
|---|---|---:|
| Model parameter | Weight | Yes |
| Model parameter | Bias | Yes |
| Generation setting | Temperature | No |
| Generation setting | Top-P | No |
| Generation setting | Output limit | No |

---

# 5️⃣ Inference

## 🚀 What Is Inference?

Inference is the process of using a trained model to produce predictions or outputs for input data.

### 🧒 Layman Example

Training:

```text
Student studies
```

Inference:

```text
Student answers exam question
```

### 💻 LLM Inference

```text
Prompt
 ↓
Tokenization
 ↓
Model
 ↓
Logits
 ↓
Decoding
 ↓
Next token
 ↓
Model again
 ↓
Next token
 ↓
...
 ↓
Complete response
```

### 🔄 Autoregressive Generation

For many decoder-style language models:

```text
Prompt
  ↓
Predict next token
  ↓
Append token
  ↓
Predict next token
  ↓
Append token
  ↓
Repeat
```

Example:

```text
Input:
"The sky is"

Model:
"blue"

Now:
"The sky is blue"

Model:
"today"

Now:
"The sky is blue today"
```

The exact generated sequence depends on model behavior and decoding settings.

---

# 6️⃣ Temperature

## 🌡️ What Is Temperature?

Temperature is a generation setting that changes how sharply or broadly the model's token-selection distribution is used during sampling in APIs that support it.

A simplified mathematical view is:

```text
Pᵢ = softmax(logitᵢ / T)
```

Where:

- `logitᵢ` = model score for token `i`
- `T` = temperature
- `Pᵢ` = resulting probability

### 🧒 Layman Example

Imagine choosing the next word from a set of options.

Low temperature:

```text
Option A ██████████
Option B ██
Option C █
```

Higher temperature can flatten the distribution:

```text
Option A █████
Option B ████
Option C ███
```

This is conceptual; exact behavior depends on the implementation.

### 🎯 Tester Purpose

Temperature testing can investigate:

```text
Consistency
Variation
Creativity
Instruction following
Error rate
```

### ⚠️ Important

Do not say:

> "High temperature always creates better creativity."

Instead say:

> "Higher temperature can increase variation in token sampling, depending on the model and implementation."

---

# 7️⃣ Top-P

## 🎯 What Is Top-P?

Top-P, also called **nucleus sampling**, limits sampling to a dynamically selected set of high-probability tokens whose cumulative probability reaches a specified threshold.

Example:

```text
Token A = 0.50
Token B = 0.25
Token C = 0.15
Token D = 0.07
Token E = 0.03
```

If:

```text
Top-P = 0.90
```

then the candidate set conceptually includes tokens until cumulative probability reaches the threshold:

```text
A = 0.50
B = 0.25
C = 0.15

Total = 0.90
```

The exact candidate behavior depends on the implementation.

### 🧒 Layman Example

Imagine selecting restaurants.

Instead of considering every restaurant in the city, keep adding the best candidates until you have covered 90% of the likely choices.

### 🌡️ Temperature vs Top-P

| Temperature | Top-P |
|---|---|
| Changes distribution sharpness | Restricts candidate probability mass |
| Often affects variability | Dynamically selects candidate set |
| Works on probabilities/logits | Works on cumulative probability |
| Model/API dependent | Model/API dependent |

### ⚠️ Testing Rule

When comparing temperature and Top-P, avoid changing both at the same time unless that is the experiment you explicitly intend to run.

---

# 8️⃣ System Prompt

## 🧑‍💼 What Is a System Prompt?

A system prompt is an instruction layer used by an application/model interface to establish high-level behavior, policies, role, or constraints.

Example:

```text
You are a cybersecurity training assistant.
Explain concepts clearly and safely.
```

### 🧒 Layman Example

Think of a manager giving an employee operating instructions before work starts.

```text
Manager:
"Follow these rules while handling customer requests."
```

### 🏗️ Conceptual Flow

```text
System Instructions
       ↓
User Prompt
       ↓
Model
       ↓
Assistant Response
```

The exact instruction hierarchy and behavior depend on the model/API.

### 🧪 Tester Checks

Test:

- Instruction following
- Conflicting instructions
- Priority behavior
- Prompt injection resistance
- Safety requirements
- Output formatting

---

# 9️⃣ User Prompt

## 👤 What Is a User Prompt?

The user prompt is the input/instruction supplied by the end user.

Example:

```text
Explain Kubernetes RBAC in simple words.
```

### 🏗️ Flow

```text
User
 ↓
User Prompt
 ↓
Application
 ↓
LLM
 ↓
Response
```

### 🧪 Tester Checks

Test:

- Valid prompts
- Invalid prompts
- Empty prompts
- Long prompts
- Ambiguous prompts
- Adversarial prompts
- Injection attempts
- Special characters
- Multilingual prompts

---

# 🔟 Assistant Response

## 🤖 What Is an Assistant Response?

The assistant response is the generated output returned by the AI assistant/application.

Example:

```text
User:
What is Kubernetes?

Assistant:
Kubernetes is a platform for managing containerized applications.
```

### 🏗️ Complete Interaction

```text
┌──────────────────────┐
│   System Prompt      │
└──────────┬───────────┘
           │
           ▼
┌──────────────────────┐
│    User Prompt       │
└──────────┬───────────┘
           │
           ▼
┌──────────────────────┐
│        LLM           │
└──────────┬───────────┘
           │
           ▼
┌──────────────────────┐
│ Assistant Response   │
└──────────────────────┘
```

The application may additionally insert:

- Conversation history
- Retrieved context
- Tool results
- Safety instructions
- Structured output requirements

---

# 1️⃣1️⃣ Hallucination

## 👻 What Is an AI Hallucination?

A hallucination is an output containing information that is unsupported, fabricated, or factually incorrect.

Example:

```text
User:
Who invented Kubernetes?

Bad response:
Kubernetes was invented by XYZ in 1998.
```

If the statement is false, it is a hallucination.

### 🧒 Layman Example

Imagine a student who doesn't know an answer but confidently invents one.

```text
"I don't know"
        ↓
would be honest

"I know!"
        ↓
but gives invented information
        ↓
Hallucination
```

### 🏗️ Why Can Hallucinations Happen?

LLMs are trained to model/generate sequences, not to guarantee truth for every statement.

Potential contributors include:

- Insufficient context
- Ambiguous prompts
- Missing knowledge
- Retrieval errors
- Conflicting context
- Model limitations
- Sampling behavior
- Poor application design

### 🛡️ How Can We Reduce Risk?

```text
User
 ↓
Query
 ↓
Retrieve trusted information
 ↓
Rerank
 ↓
Provide context
 ↓
LLM
 ↓
Answer
 ↓
Validation
```

Additional controls can include:

- RAG
- Source citations
- Structured outputs
- Tool use
- Verification
- Human review
- Evaluation suites
- Guardrails

No single technique guarantees zero hallucinations.

---

# 1️⃣2️⃣ Model Non-Determinism

## 🎲 What Is Model Non-Determinism?

Model non-determinism means that repeated requests can sometimes produce different outputs even when the prompt appears unchanged.

### 🧒 Layman Example

Ask a person:

```text
"Give me a restaurant recommendation."
```

They might answer:

```text
Restaurant A
```

Later:

```text
Restaurant B
```

Both could be reasonable.

LLM generation can similarly vary.

### 🧠 Why Can Responses Differ?

Possible contributors include:

```text
Sampling
Temperature
Top-P
Randomness
Backend implementation
Parallelism
Model/version changes
Provider behavior
```

The exact causes vary by system.

### 📊 Example

```text
Prompt:
Write a slogan for a cloud-security company.

Run 1:
"Secure Every Cloud."

Run 2:
"Protect What Runs in the Cloud."

Run 3:
"Confidence Across Every Cloud."
```

Different does not automatically mean incorrect.

---

# 🔗 How Everything Communicates

Here is the most important architecture in this README:

```text
                         👤 USER
                            │
                            ▼
                      User Prompt
                            │
                            ▼
                    Application API
                            │
             ┌──────────────┴──────────────┐
             ▼                             ▼
      System Prompt                  Conversation
             │                         History
             └──────────────┬──────────────┘
                            ▼
                     Context Builder
                            │
                            ▼
                       Tokenizer
                            │
                            ▼
                         Tokens
                            │
                            ▼
                    Context Window
                            │
                            ▼
                     LLM Inference
                            │
                            ▼
                    Model Parameters
                     / Weights
                            │
                            ▼
                         Logits
                            │
                            ▼
                 Temperature / Top-P
                     Decoding
                            │
                            ▼
                       Next Token
                            │
                            ▼
                         Repeat
                            │
                            ▼
                  Assistant Response
                            │
             ┌──────────────┴──────────────┐
             ▼                             ▼
       Safety / Validation             Logging
             │
             ▼
                           USER
```

---

# 🧪 Complete Tester Workflow

A tester should think in stages.

```text
                    TEST START
                        │
                        ▼
                  Define Prompt
                        │
                        ▼
                 Define Expected
                   Behavior
                        │
                        ▼
                  Send Request
                        │
                        ▼
                 Capture Output
                        │
          ┌─────────────┼─────────────┐
          ▼             ▼             ▼
      Correctness    Relevance      Safety
          │             │             │
          └─────────────┼─────────────┘
                        ▼
                    Evaluate
                        │
                        ▼
                 PASS / FAIL /
                    REVIEW
```

---

# 📊 Important Metrics

Depending on the application, measure:

## ⏱️ Latency

```text
Request → Response
```

Possible measurements:

- Time to first token
- Total response latency

---

## 💰 Cost

Measure relevant:

- Input tokens
- Output tokens
- Model/API pricing
- Retrieval costs
- Compute costs

---

## 🎯 Quality

Possible dimensions:

```text
Correctness
Relevance
Completeness
Faithfulness
Groundedness
Safety
Instruction Following
```

---

## 🔁 Consistency

Run the same test multiple times.

```text
Run 1 → Response A
Run 2 → Response A
Run 3 → Response B
Run 4 → Response A
```

Then investigate whether the observed variation is acceptable.

---

# 🧪 Test Case Examples

## TC-001 — Token Boundary Test

### Objective

Verify that the application correctly handles long and unusual text.

### Test Data

```text
Normal sentence
Very long sentence
Numbers
Special characters
Unicode
Code
URLs
```

### Expected

```text
No unexpected truncation
No application crash
Correct token/context handling
```

---

# TC-002 — Context Window Test

```text
Short context
Medium context
Near-limit context
Over-limit context
```

Test:

```text
Does the application fail safely?
Does it truncate?
Does it summarize?
Does it reject the request?
```

The expected behavior should be explicitly defined by the application.

---

# TC-003 — Temperature Test

```text
Prompt = SAME
Model = SAME

Temperature:
0.2
0.7
1.0
```

Measure:

```text
Variation
Correctness
Relevance
Instruction following
```

---

# TC-004 — Top-P Test

```text
Prompt = SAME
Temperature = CONTROLLED

Top-P:
Low
Medium
High
```

Compare:

```text
Response diversity
Quality
Stability
```

Use values supported by the selected model/API.

---

# TC-005 — Hallucination Test

Prompt:

```text
Explain a deliberately obscure or unsupported fact.
```

Evaluate:

```text
Does the model admit uncertainty?
Does it invent a confident answer?
Can it cite/ground the answer?
```

---

# TC-006 — Non-Determinism Test

Run:

```text
Same prompt × 10
```

Capture:

```text
Response
Latency
Parameters
Model version/identifier if available
Timestamp
```

Then compare outputs.

---

# 🛡️ Security Testing

LLM systems introduce additional security concerns.

## Prompt Injection

```text
Trusted Instruction
       +
Untrusted User Input
       ↓
Potential Instruction Conflict
```

Test whether untrusted input can override application rules.

---

## Sensitive Data Leakage

Test whether the model/application reveals:

- Secrets
- Credentials
- PII
- Internal prompts
- Confidential documents

---

## RAG Poisoning

```text
Document
 ↓
Index
 ↓
Embedding
 ↓
Retriever
 ↓
LLM
```

Test whether malicious or incorrect documents can manipulate answers.

---

## Tool Abuse

If the LLM can call tools:

```text
LLM
 ↓
Tool
 ↓
Database / API / Cloud
```

test:

- Authorization
- Input validation
- Least privilege
- Dangerous commands
- Data exfiltration
- Audit logging

---

# 🐛 Common Problems

## ❌ "One token equals one word"

Usually false.

Tokenization depends on the tokenizer.

---

## ❌ "Temperature is a model parameter"

Not in the same sense as weights.

Temperature is usually a **generation/API setting**.

---

## ❌ "More temperature means more intelligence"

False.

Temperature affects generation behavior, not model intelligence.

---

## ❌ "Hallucination means the model is broken"

Not necessarily.

Hallucination is a known failure mode/risk of generative systems and must be managed through model selection, grounding, application design, evaluation, and monitoring.

---

## ❌ "Same prompt must always return exactly the same answer"

Not necessarily.

Generation can be non-deterministic depending on sampling and system implementation.

---

## ❌ "Top-P and temperature are the same"

No.

They influence generation differently.

---

## ❌ "Context window is only the user's prompt"

No.

Depending on the API/application, context can include system instructions, conversation history, retrieved content, tool results, and other inputs.

---

# 📖 Key Definitions

| Term | Definition |
|---|---|
| Token | Unit of text representation processed/generated by a language model. |
| Tokenization | Conversion of text into tokens and token IDs. |
| Context Window | Supported amount of contextual tokenized information for a model/request. |
| Model Parameter | Learned numerical value such as a weight or bias. |
| API Generation Parameter | Request setting such as temperature or Top-P. |
| Inference | Using a trained model to generate predictions/outputs for input. |
| Temperature | Sampling-related setting that changes the sharpness of the token probability distribution. |
| Top-P | Nucleus sampling method that limits candidates to a cumulative probability mass. |
| System Prompt | High-level application/model instructions establishing desired behavior or constraints. |
| User Prompt | Input/instruction provided by the user. |
| Assistant Response | Output generated by the AI assistant. |
| Hallucination | Unsupported, fabricated, or incorrect generated information. |
| Non-Determinism | Variation in outputs across otherwise similar repeated requests. |
| Logit | A raw model score used before conversion to probabilities. |
| Sampling | Selecting generated tokens according to a decoding strategy/distribution. |
| Decoding | Process used to turn model output scores into generated tokens. |
| Embedding | Numerical vector representation of data. |
| RAG | Retrieval-Augmented Generation: retrieve external context and provide it to a generative model. |

---

# 🧠 The Complete Mental Model

```text
                    USER
                     │
                     ▼
                 USER PROMPT
                     │
                     ▼
                TOKENIZATION
                     │
                     ▼
                   TOKENS
                     │
                     ▼
              CONTEXT WINDOW
                     │
                     ▼
              TRANSFORMER / LLM
                     │
                     ▼
                  LOGITS
                     │
             ┌───────┴────────┐
             ▼                ▼
        Temperature          Top-P
             └───────┬────────┘
                     ▼
                  DECODING
                     │
                     ▼
                NEXT TOKEN
                     │
                     ▼
                 NEXT TOKEN
                     │
                     ▼
                    ...
                     │
                     ▼
            ASSISTANT RESPONSE
                     │
          ┌──────────┼──────────┐
          ▼          ▼          ▼
      Evaluation   Safety    Logging
          │
          ▼
       Final User
```

---

# 🚀 Learning Path

```text
01
Tokens
 ↓
02
Tokenization
 ↓
03
Embeddings
 ↓
04
Context Window
 ↓
05
Transformer
 ↓
06
Inference
 ↓
07
Logits
 ↓
08
Temperature
 ↓
09
Top-P
 ↓
10
System Prompt
 ↓
11
User Prompt
 ↓
12
Assistant Response
 ↓
13
Hallucination
 ↓
14
Non-Determinism
 ↓
15
Prompt Engineering
 ↓
16
RAG
 ↓
17
LLM Evaluation
 ↓
18
LLM Testing
 ↓
19
LLM Security
 ↓
20
AI Quality Engineering
```

---

# 🏁 Final One-Minute Explanation

> **A user sends a prompt to an AI application. The application constructs the model context, tokenizes the input, and sends it to the LLM for inference. The model processes the token sequence and produces scores for possible next tokens. A decoding strategy, potentially influenced by settings such as temperature and Top-P, selects generated tokens. The process repeats until the response is complete. The application can then validate, evaluate, secure, log, and return the assistant response.**

---

# ⭐ Tester Golden Rules

```text
1. Don't test only HTTP 200.
2. Test the actual AI behavior.
3. Test correctness.
4. Test relevance.
5. Test instruction following.
6. Test safety.
7. Test groundedness where applicable.
8. Test context limits.
9. Test repeated runs.
10. Test latency and cost.
11. Test hallucination risk.
12. Test prompt injection.
13. Test model/version changes.
14. Build regression suites.
15. Automate evaluation.
```

---

# 🏆 VishwaTech-Labs

### 🤖 AI Testing
### 🧠 LLM Testing
### 🧪 AI Quality Engineering
### 🔐 AI Security
### ⚙️ MLOps
### 🚀 DevSecOps

> **Understand the model → Understand the application → Test the behavior → Measure quality → Secure the system**
