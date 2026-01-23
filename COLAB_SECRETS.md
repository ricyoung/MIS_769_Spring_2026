# Colab Secrets - Standardized Naming Convention

When using Google Colab's Secrets feature (the key icon in the left sidebar), please use these **exact names** so code works consistently across all assignments.

## How to Add Secrets in Colab

1. Click the **key icon** (🔑) in the left sidebar
2. Click **"Add new secret"**
3. Use the exact name from the table below
4. Paste your token/key as the value
5. Toggle **"Notebook access"** to ON

## Standard Secret Names

| Secret Name | Service | How to Get It |
|-------------|---------|---------------|
| `HF_TOKEN` | HuggingFace | [huggingface.co/settings/tokens](https://huggingface.co/settings/tokens) |
| `OPENAI_API_KEY` | OpenAI | [platform.openai.com/api-keys](https://platform.openai.com/api-keys) |
| `ANTHROPIC_API_KEY` | Claude/Anthropic | [console.anthropic.com](https://console.anthropic.com/) |
| `GOOGLE_API_KEY` | Google AI / Gemini | [aistudio.google.com/apikey](https://aistudio.google.com/apikey) |
| `COHERE_API_KEY` | Cohere | [dashboard.cohere.com/api-keys](https://dashboard.cohere.com/api-keys) |
| `PINECONE_API_KEY` | Pinecone (Vector DB) | [app.pinecone.io](https://app.pinecone.io/) |
| `WANDB_API_KEY` | Weights & Biases | [wandb.ai/authorize](https://wandb.ai/authorize) |

## Accessing Secrets in Code

```python
from google.colab import userdata

# HuggingFace
hf_token = userdata.get('HF_TOKEN')

# OpenAI
openai_key = userdata.get('OPENAI_API_KEY')

# Anthropic
anthropic_key = userdata.get('ANTHROPIC_API_KEY')

# Google AI
google_key = userdata.get('GOOGLE_API_KEY')
```

## HuggingFace Login Example

```python
from google.colab import userdata
from huggingface_hub import login

login(token=userdata.get('HF_TOKEN'))
```

## OpenAI Client Example

```python
from google.colab import userdata
from openai import OpenAI

client = OpenAI(api_key=userdata.get('OPENAI_API_KEY'))
```

## Why This Matters

- **Consistency**: Everyone's code works the same way
- **Security**: Secrets never appear in your notebook code
- **Sharing**: You can share notebooks without exposing keys
- **Grading**: TAs can run your code with their own keys

## Common Mistakes to Avoid

| Wrong | Right |
|-------|-------|
| `HuggingFace_token` | `HF_TOKEN` |
| `hf_token` | `HF_TOKEN` |
| `HUGGINGFACE_TOKEN` | `HF_TOKEN` |
| `openai_key` | `OPENAI_API_KEY` |
| `OPENAI_KEY` | `OPENAI_API_KEY` |

## Free Tiers

Most services have free tiers sufficient for coursework:
- **HuggingFace**: Free (required for this course)
- **OpenAI**: Pay-as-you-go (optional)
- **Google AI**: Free tier available
- **Cohere**: Free tier available
- **Anthropic**: Pay-as-you-go (optional)

---

*Use `HF_TOKEN` for all HuggingFace assignments in this course.*
