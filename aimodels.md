# 🤖 LLM / AI Models --- Public, Open-Weight & Private Models

![AI](https://img.shields.io/badge/AI-LLM%20Models-6A5ACD?style=for-the-badge)
![Testing](https://img.shields.io/badge/AI%20Testing-Quality%20Engineering-00A896?style=for-the-badge)
![Python](https://img.shields.io/badge/Python-3.x-3776AB?style=for-the-badge&logo=python&logoColor=white)
![API](https://img.shields.io/badge/API-LLM%20Integration-FF6F00?style=for-the-badge)
![GitHub](https://img.shields.io/badge/GitHub-README-181717?style=for-the-badge&logo=github&logoColor=white)

> 📘 **Purpose:** A practical reference for understanding major
> LLM/model families, their use cases, advantages, limitations, where to
> obtain model access, and where to create API keys.

------------------------------------------------------------------------

## 🧭 1. First: What Is an AI Model?

An **AI model** is a trained mathematical/computational system that has
learned patterns from data and can perform tasks such as classification,
prediction, generation, reasoning, vision, speech, embeddings, or code
generation.

For LLM applications, the simplified flow is:

``` text
User / Application
        |
        v
     Prompt
        |
        v
   API / Runtime
        |
        v
      Model
        |
        v
 Generated Output
```

### 🔑 Important distinction

  -----------------------------------------------------------------------
  Item                                Meaning
  ----------------------------------- -----------------------------------
  **Model**                           The trained AI system that performs
                                      the task

  **Model ID**                        Name/identifier used to select a
                                      model through an API

  **API**                             Interface used by software to
                                      communicate with a hosted model

  **API Key**                         Secret credential used to
                                      authenticate API requests

  **Weights**                         Learned numerical parameters of a
                                      model

  **Inference**                       Running the trained model to
                                      produce an output

  **Prompt**                          Instructions/input sent to the
                                      model
  -----------------------------------------------------------------------

------------------------------------------------------------------------

# 🏗️ 2. Major Model Categories

  ----------------------------------------------------------------------------------
  Category               What it does            Examples /        Typical use
                                                 Families          
  ---------------------- ----------------------- ----------------- -----------------
  🧠 **LLM / Language**  Understands and         GPT, Claude,      Chat, reasoning,
                         generates text          Gemini, Llama,    summarization
                                                 Mistral, Qwen     

  👁️ **Vision**          Understands images      GPT               Image analysis,
                                                 vision-capable    OCR, visual QA
                                                 models, Gemini,   
                                                 Claude vision,    
                                                 Llama vision      

  🔀 **Multimodal**      Handles multiple        GPT, Gemini,      Text + image +
                         input/output modalities Claude, Llama     audio workflows

  💻 **Code models**     Generates/understands   GPT, Claude,      Coding, review,
                         code                    Qwen, DeepSeek,   testing
                                                 Code-oriented     
                                                 models            

  🔢 **Embedding         Converts data into      OpenAI            RAG, semantic
  models**               vectors                 embeddings,       search
                                                 Gemini            
                                                 embeddings,       
                                                 open-source       
                                                 embedding         
                                                 families          

  🎙️ **Speech / Audio**  Speech-to-text,         GPT               Transcription,
                         text-to-speech, audio   audio/realtime    voice agents
                         understanding           families, Gemini  
                                                 audio, Whisper    

  🎨 **Image             Generates or edits      GPT image         Design,
  generation**           images                  families, Imagen, marketing,
                                                 FLUX, Stable      visualization
                                                 Diffusion         

  🛡️ **Safety / Guard    Detects unsafe or       Llama Guard and   AI safety
  models**               policy-sensitive        similar safety    filtering
                         content                 models            

  🧩 **Small / Edge      Runs with lower compute Llama small       Local/edge AI
  models**                                       models, Gemma,    
                                                 Phi, Qwen small   
                                                 variants          

  🏢                     Models deployed under   Self-hosted       Sensitive
  **Enterprise/private   organizational control  open-weight       workloads,
  models**                                       models or managed private inference
                                                 enterprise        
                                                 deployments       
  ----------------------------------------------------------------------------------

------------------------------------------------------------------------

# 🌎 3. Public / Hosted AI Models

> **Public/hosted** here means models exposed through a provider's
> cloud/API. It does **not** mean the model weights are publicly
> downloadable.

  -------------------------------------------------------------------------------------------------------------
  Provider        Major       Example current        Main use cases  Pros                Cons
                  family      families                                                   
  --------------- ----------- ---------------------- --------------- ------------------- ----------------------
  🟢 **OpenAI**   GPT         GPT-6 Astra, GPT-5.6   Reasoning,      Strong general      API cost, provider
                              Sol/Terra/Luna,        coding, agents, capability, mature  dependency,
                              image/realtime/audio   multimodal, AI  API ecosystem       hosted-data
                              families               apps                                considerations

  🔵 **Google**   Gemini      Gemini family          Multimodal,     Strong multimodal   API/model availability
                                                     long-context,   ecosystem, Google   and pricing vary
                                                     reasoning,      integration         
                                                     agents                              

  🟣              Claude      Claude family          Coding,         Strong              Hosted service, API
  **Anthropic**                                      reasoning,      writing/coding and  cost, model
                                                     enterprise      long-context        availability varies
                                                     assistants      workflows           

  🟠 **Mistral    Mistral     Large/Medium/Small     Chat, coding,   API + open-weight   Capability varies by
  AI**                        families               agents, RAG     ecosystem, flexible model; licensing
                                                                     deployment options  differs by model

  🟦 **Meta**     Llama       Llama 3.x / Llama 4    Local           Open-weight         Hardware, licensing
                                                     inference,      ecosystem and many  and self-hosting
                                                     enterprise AI,  deployment choices  complexity
                                                     research,                           
                                                     agents                              

  🟨 **Alibaba**  Qwen        Qwen family            Multilingual,   Strong open-model   Deployment/licensing
                                                     coding,         ecosystem           differences by model
                                                     reasoning                           

  🟥 **DeepSeek** DeepSeek    DeepSeek family        Reasoning,      Strong open-model   Provider/model
                                                     coding, general ecosystem and       availability and
                                                     LLM tasks       cost-focused        licensing vary
                                                                     options             

  🟪 **Cohere**   Command     Command family         Enterprise RAG, Enterprise/search   More specialized
                                                     search, agents  focus               ecosystem

  🟧 **xAI**      Grok        Grok family            General chat,   Strong              Access, pricing and
                                                     reasoning,      real-time/social    product availability
                                                     applications    ecosystem           vary
                                                                     integrations        
  -------------------------------------------------------------------------------------------------------------

> ⚠️ **Model catalogs change frequently.** Always verify the provider's
> current model catalog before putting a model ID into production.

------------------------------------------------------------------------

# 🔓 4. Open-Weight / Downloadable Models

**Open-weight** means model weights can be obtained under the applicable
license and potentially run through supported runtimes. It does **not**
automatically mean unrestricted open source.

  ---------------------------------------------------------------------------------------------------
  Family         Organization   Common       Best suited for    Pros          Cons
                                deployment                                    
                                style                                         
  -------------- -------------- ------------ ------------------ ------------- -----------------------
  🦙 **Llama**   Meta           Local GPU /  General AI, RAG,   Large         Hardware + license
                                cloud /      agents             ecosystem,    considerations
                                hosted                          many sizes    
                                providers                                     

  🌪️ **Mistral / Mistral AI     Local /      General AI,        Efficient     Model-specific
  Mixtral**                     cloud        coding, RAG        options,      licenses/capabilities
                                                                open-weight   
                                                                choices       

  💠 **Qwen**    Alibaba        Local /      Multilingual,      Broad family  License varies by model
                                cloud        coding, reasoning  and sizes     

  🔥             DeepSeek       Local /      Reasoning/coding   Strong        Hardware and
  **DeepSeek**                  hosted                          open-model    model-version
                                                                ecosystem     considerations

  💎 **Gemma**   Google         Local /      Smaller AI         Lightweight   Smaller models have
                                cloud        applications       choices       different capability
                                                                              ceilings

  🧠 **Phi**     Microsoft      Local / edge Small-model        Efficient and Not designed to replace
                                             applications       compact       every frontier model
                                                                options       

  🌊 **Stable    Stability AI / Local /      Image generation   Huge          Image quality and
  Diffusion /    ecosystem      cloud                           ecosystem     license depend on
  related**                                                                   version
  ---------------------------------------------------------------------------------------------------

Meta's official Llama resources provide direct and partner-based access,
including Hugging Face and cloud partners.
citeturn0search0turn0search1

------------------------------------------------------------------------

# 🔐 5. Private AI Models --- What Does "Private" Mean?

There are two common meanings:

### A. Private deployment

You take an open-weight model and run it inside:

``` text
Your Data Center
       |
       v
GPU Servers
       |
       v
Model Runtime
       |
       v
Private LLM API
```

Examples:

-   Llama
-   Mistral
-   Qwen
-   DeepSeek
-   Gemma
-   Phi

### B. Private enterprise access

A company uses a managed cloud AI service with enterprise controls,
private networking, access policies, logging, and data-governance
controls.

Examples include:

-   Azure-hosted AI services
-   Google Cloud Vertex AI
-   AWS managed model services
-   Enterprise deployments from model providers

------------------------------------------------------------------------

# 🏢 6. Public vs Private AI

  -----------------------------------------------------------------------
  Feature                 🌎 Public / Hosted API  🔐 Private /
                                                  Self-hosted
  ----------------------- ----------------------- -----------------------
  Infrastructure          Provider                Your organization /
                                                  cloud

  Setup                   Easy                    More complex

  GPU management          Provider                You manage it

  Scaling                 Usually easier          You manage scaling

  Data governance         Provider controls +     Greater infrastructure
                          contract/settings       control

  Cost model              Usually API/token based GPU + infrastructure +
                                                  operations

  Customization           API/config dependent    High

  Model weights           Usually unavailable     Available for
                                                  applicable open-weight
                                                  models

  Maintenance             Provider                Your team

  Best for                Fast development        Sensitive/controlled
                                                  workloads
  -----------------------------------------------------------------------

------------------------------------------------------------------------

# 🔑 7. Where Do I Get the Models?

## 🟢 OpenAI

**Model catalog:**\
https://developers.openai.com/api/docs/models

**API quickstart:**\
https://platform.openai.com/docs/quickstart/make-your-first-api-request

**API key:**\
https://platform.openai.com/api-keys

OpenAI's current model catalog includes GPT-6 Astra and GPT-5.6
Sol/Terra/Luna families plus image, realtime, audio and other
specialized models. citeturn0search6turn0search14

------------------------------------------------------------------------

## 🔵 Google Gemini

**Models / API documentation:**\
https://ai.google.dev/gemini-api/docs

**API key:**\
https://aistudio.google.com/apikey

Google's Gemini API documentation explains that an API key is required
for authentication and that Google AI Studio can create/manage keys.
citeturn0search9turn0search12

------------------------------------------------------------------------

## 🟣 Anthropic Claude

**Models:**\
https://docs.anthropic.com/en/docs/about-claude/models

**API console / keys:**\
https://console.anthropic.com/

Use the official Anthropic documentation to verify the currently
available Claude model IDs and API requirements.

------------------------------------------------------------------------

## 🟠 Mistral AI

**Models / documentation:**\
https://docs.mistral.ai/

**Studio:**\
https://console.mistral.ai/

**API keys:**\
https://console.mistral.ai/api-keys/

Mistral's Studio provides API access, model testing, a Playground,
evaluations and API-key management. citeturn0search3turn0search4

------------------------------------------------------------------------

## 🦙 Meta Llama

**Official Llama resources:**\
https://ai.meta.com/llama/

**Llama downloads:**\
https://www.llama.com/llama-downloads/

**Official GitHub:**\
https://github.com/meta-llama/

Meta provides direct and partner access to Llama models, including
Hugging Face and cloud partners. citeturn0search0turn0search8

------------------------------------------------------------------------

## 🤗 Hugging Face

**Models:**\
https://huggingface.co/models

**Access tokens:**\
https://huggingface.co/settings/tokens

Hugging Face is a major distribution and hosting ecosystem for
open/open-weight models from many organizations.

------------------------------------------------------------------------

# 🧰 8. Where Can I Run Open-Weight Models?

  Platform / Runtime   Purpose
  -------------------- ------------------------------------
  🤗 Hugging Face      Discover/download/host models
  🖥️ Ollama            Easy local model execution
  ⚡ vLLM              High-performance model serving
  🦙 llama.cpp         Efficient local inference
  🐳 Docker            Package model-serving environments
  ☁️ AWS               Cloud GPU/model deployment
  ☁️ Azure             Enterprise/cloud AI deployment
  ☁️ Google Cloud      GPU and managed AI deployment
  ☁️ OCI               Cloud AI infrastructure

------------------------------------------------------------------------

# 🔌 9. API Key vs Model Download

This distinction is **very important** for AI testers.

### Hosted model

``` text
You
 |
 | API KEY
 v
Provider API
 |
 v
MODEL
 |
 v
Response
```

You normally **do not download the model weights**.

### Open-weight model

``` text
Model Repository
       |
       | Download
       v
Your GPU / Cloud
       |
       v
Model Runtime
       |
       v
Your Application
```

You may not need a provider inference API key if you run the model
completely locally, although you may need authentication to download the
model or use a hosted service.

------------------------------------------------------------------------

# 🧪 10. For Your AI Testing Course --- Recommended Models

If your objective is **learning LLM testing**, don't try to test 100
models immediately.

Start with a small cross-provider matrix:

  Lab         Model family      What to learn
  ----------- ----------------- ------------------------------------
  🧪 Lab 1    OpenAI GPT        Hosted API testing
  🧪 Lab 2    Gemini            Cross-provider testing
  🧪 Lab 3    Claude            Response-quality comparison
  🧪 Lab 4    Mistral           API + parameter testing
  🧪 Lab 5    Llama             Open-weight/local testing
  🧪 Lab 6    Qwen              Multilingual/coding comparison
  🧪 Lab 7    Embedding model   RAG/semantic-search testing
  🧪 Lab 8    Vision model      Image/vision testing
  🧪 Lab 9    Safety model      AI safety testing
  🧪 Lab 10   Multiple models   LLM benchmark/regression framework

------------------------------------------------------------------------

# 🎯 11. The Tester View

Eventually your framework should look like:

``` text
                    AI TEST FRAMEWORK
                           |
          +----------------+----------------+
          |                |                |
       OpenAI           Gemini           Claude
          |                |                |
          +----------------+----------------+
                           |
                    Same Test Suite
                           |
          +----------------+----------------+
          |                |                |
     Correctness       Relevance        Safety
          |                |                |
          +----------------+----------------+
                           |
                      Evaluation
                           |
                       Scorecard
                           |
                    PASS / FAIL
                           |
                        CI/CD
```

That is much more powerful than simply asking:

> "Which AI model is best?"

The better tester question is:

> **"Which model performs best for my specific workload under my defined
> quality, security, cost, latency and compliance requirements?"**

------------------------------------------------------------------------

# 🔒 12. API Key Security Rules

Never commit:

``` text
OPENAI_API_KEY=sk-...
GEMINI_API_KEY=...
ANTHROPIC_API_KEY=...
```

to GitHub.

Use:

``` text
Environment variables
        +
.env locally
        +
.gitignore
        +
CI/CD Secrets
        +
Secrets Manager / Vault
```

Example:

``` python
import os

api_key = os.environ["OPENAI_API_KEY"]
```

And add:

``` text
.env
*.key
secrets/
```

to `.gitignore`.

------------------------------------------------------------------------

# 📚 13. Useful Official Links

  -------------------------------------------------------------------------------------------------------------------------
  Provider                Models                                                   API / Keys
  ----------------------- -------------------------------------------------------- ----------------------------------------
  OpenAI                  https://developers.openai.com/api/docs/models            https://platform.openai.com/api-keys

  Google Gemini           https://ai.google.dev/gemini-api/docs                    https://aistudio.google.com/apikey

  Anthropic               https://docs.anthropic.com/en/docs/about-claude/models   https://console.anthropic.com/

  Mistral                 https://docs.mistral.ai/                                 https://console.mistral.ai/

  Meta Llama              https://ai.meta.com/llama/                               Model access/download via Meta

  Hugging Face            https://huggingface.co/models                            https://huggingface.co/settings/tokens

  Ollama                  https://ollama.com/                                      Local runtime --- normally no provider
                                                                                   API key

  vLLM                    https://github.com/vllm-project/vllm                     Self-hosted inference
  -------------------------------------------------------------------------------------------------------------------------

------------------------------------------------------------------------

# 🚀 14. Recommended Learning Path

``` text
                 AI / LLM TESTING
                       |
                       v
                1. Understand Models
                       |
                       v
                2. Understand APIs
                       |
                       v
                3. Get API Keys
                       |
                       v
                4. Python API Calls
                       |
                       v
                5. Prompt Testing
                       |
                       v
                6. Parameter Testing
                       |
                       v
                7. Temperature Testing
                       |
                       v
                8. Response Evaluation
                       |
                       v
                9. Hallucination Testing
                       |
                       v
               10. RAG Testing
                       |
                       v
               11. Safety Testing
                       |
                       v
               12. LLM Benchmarking
                       |
                       v
               13. Regression Testing
                       |
                       v
               14. CI/CD Integration
                       |
                       v
             🏆 AI QUALITY ENGINEERING
```

> **Key takeaway:** There isn't one fixed number of AI models. Model
> catalogs are continuously changing, and a provider may expose many
> model variants for text, reasoning, vision, audio, image generation,
> embeddings, and other tasks. For a course, organize them by **model
> family + capability + deployment type**, rather than trying to
> memorize every model ID.

This README is designed to be pasted directly into a GitHub repository
and uses GitHub-compatible badges, tables, emojis, code blocks, and
hyperlinks. The official provider documentation should always be checked
before teaching a specific model ID or API parameter because those
details change over time.
