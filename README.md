<div align="center">

# omni-llm

[![Docs](https://github.com/adish333/omni-llm/actions/workflows/docs.yaml/badge.svg)](https://github.com/adish333/omni-llm/actions/workflows/docs.yaml/)
[![Linting](https://github.com/adish333/omni-llm/actions/workflows/lint.yaml/badge.svg)](https://github.com/adish333/omni-llm/actions/workflows/lint.yaml/)
[![Unit Tests](https://github.com/adish333/omni-llm/actions/workflows/tests-unit.yaml/badge.svg)](https://github.com/adish333/omni-llm/actions/workflows/tests-unit.yaml/)
[![Integration Tests](https://github.com/adish333/omni-llm/actions/workflows/tests-integration.yaml/badge.svg)](https://github.com/adish333/omni-llm/actions/workflows/tests-integration.yaml/)

![Python 3.11+](https://img.shields.io/badge/python-3.11%2B-blue.svg)
[![PyPI](https://img.shields.io/pypi/v/omni-llm)](https://pypi.org/project/omni-llm/)

A unified interface for interacting with multiple LLM providers - supporting 20+ providers out of the box.

</div>

## [Documentation](https://github.com/adish333/omni-llm)

## [Supported Providers](https://github.com/adish333/omni-llm/blob/main/docs/providers.md)

## Key Features

`omni-llm` offers:
- **Simple, unified interface** - one function for all providers, switch models with just a string change
- **Developer friendly** - full type hints for better IDE support and clear, actionable error messages
- **Leverages official provider SDKs** when available, reducing maintenance burden and ensuring compatibility
- **Stays framework-agnostic** so it can be used across different projects and use cases
- **Actively maintained** - used in production systems ensuring continued support
- **No Proxy or Gateway server required** so you don't need to deal with setting up any other service to talk to whichever LLM provider you need.

## Motivation

The landscape of LLM provider interfaces presents a fragmented ecosystem with several challenges that `omni-llm` aims to address:

**The Challenge with API Standardization:**

While the OpenAI API has become the de facto standard for LLM provider interfaces, providers implement slight variations. Some providers are fully OpenAI-compatible, while others may have different parameter names, response formats, or feature sets. This creates a need for light wrappers that can gracefully handle these differences while maintaining a consistent interface.

**Existing Solutions and Their Limitations:**

- **[LiteLLM](https://github.com/BerriAI/litellm)**: While popular, it reimplements provider interfaces rather than leveraging official SDKs, which can lead to compatibility issues and unexpected behavior modifications
- **[AISuite](https://github.com/andrewyng/aisuite/issues)**: Offers a clean, modular approach but lacks active maintenance, comprehensive testing, and modern Python typing standards.
- **[Framework-specific solutions](https://github.com/agno-agi/agno/tree/main/libs/agno/agno/models)**: Some agent frameworks either depend on LiteLLM or implement their own provider integrations, creating fragmentation
- **[Proxy Only Solutions](https://openrouter.ai/)**: solutions like [OpenRouter](https://openrouter.ai/) and [Portkey](https://github.com/Portkey-AI/portkey-python-sdk) require a hosted proxy to serve as the interface between your code and the LLM provider.

## Quickstart

### Requirements

- Python 3.11 or newer
- API_KEYS to access to whichever LLM you choose to use.

### Installation

In your pip install, include the [supported providers](./docs/providers.md) that you plan on using, or use the `all` option if you want to install support for all `omni-llm` supported providers.

```bash
pip install 'omni-llm[mistral,ollama]'
```

Make sure you have the appropriate API key environment variable set for your provider. Alternatively,
you could use the `api_key` parameter when making a completion call instead of setting an environment variable.

```bash
export MISTRAL_API_KEY="YOUR_KEY_HERE"  # or OPENAI_API_KEY, etc
```

### Basic Usage

The provider_id key of the model should be specified according the [provider ids supported by omni-llm](./docs/providers.md).
The `model_id` portion is passed directly to the provider internals: to understand what model ids are available for a provider,
you will need to refer to the provider documentation.

```python
from omni_llm import completion
import os

# Make sure you have the appropriate environment variable set
assert os.environ.get('MISTRAL_API_KEY')
# Basic completion
response = completion(
    model="mistral/mistral-small-latest", # <provider_id>/<model_id>
    messages=[{"role": "user", "content": "Hello!"}]
)
print(response.choices[0].message.content)
```
