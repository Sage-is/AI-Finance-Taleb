# Taleb 🤖

Taleb is named after Nassim Nicholas Taleb, whose work on uncertainty, risk, and complex systems inspired a new way of understanding financial markets. In that spirit, Taleb is an autonomous financial research agent designed to think critically, plan strategically, and learn continuously as it analyzes real-time market data. It transforms complex financial questions into transparent, step-by-step investigations—emulating how an expert analyst works, but powered by advanced AI.

Built specifically for financial research, Taleb merges task planning, self-reflection, and live data streams into a seamless workflow. It is more than an assistant—it is a self-driven researcher that aims to bring clarity in an uncertain market environment.

100% FREE to use! No paid API keys needed for core financial data—Taleb can stary you off with free sources like Yahoo Finance and SEC EDGAR to keep insights accessible to all investors and researchers.


## Overview

Taleb takes complex financial questions and turns them into clear, step-by-step research plans. It runs those tasks using live market data, checks its own work, and refines the results until it has a confident, data-backed answer.  

**Key Capabilities:**
- **Intelligent Task Planning**: Automatically decomposes complex queries into structured research steps
- **Autonomous Execution**: Selects and executes the right tools to gather financial data
- **Self-Validation**: Checks its own work and iterates until tasks are complete
- **Real-Time Financial Data**: Access to income statements, balance sheets, cash flow statements, prices, and news
- **Multiple Data Sources**: Free (Yahoo Finance, SEC EDGAR) or paid (Financial Datasets) - your choice!
- **Flexible LLM Support**: Works with OpenAI, OpenRouter, Ollama, llama.cpp, or any OpenAI-compatible API
- **Safety Features**: Built-in loop detection and step limits to prevent runaway execution

[![Twitter Follow](https://img.shields.io/twitter/follow/virattt?style=social)](https://twitter.com/virattt)

### Prerequisites

- Python 3.10 or higher
- [uv](https://github.com/astral-sh/uv) package manager
- LLM API access (one of):
  - **OpenAI API** key (get [here](https://platform.openai.com/api-keys))
  - **OpenRouter** account (get [here](https://openrouter.ai))
  - **Local LLM** via Ollama or llama.cpp (free!)
  - Any OpenAI-compatible API
- **No financial data API key required!** (Uses free Yahoo Finance & SEC EDGAR by default)

### Installation

1. Clone the repository:
```bash
git clone https://github.com/virattt/taleb.git
cd taleb
```

2. Install dependencies with uv:
```bash
uv sync
```

3. Set up your environment variables:
```bash
# Copy the example environment file
cp env.example .env

# Edit .env and add your OpenAI API key
# OPENAI_API_KEY=your-openai-api-key
# That's it! No other API keys required.
```

### Data Sources

Taleb now supports multiple data providers, giving you flexibility based on your needs and budget:

#### Free Options (Default - No API Keys Required!)
- **Yahoo Finance** (via yfinance): Stock prices, financial statements, metrics, news
- **SEC EDGAR**: SEC filings (10-K, 10-Q, 8-K metadata)

#### Paid Options (Optional)
- **Financial Datasets API**: Premium data with analyst estimates, segmented revenues, and full filing text
  - Get API key at [financialdatasets.ai](https://financialdatasets.ai)
  - Set `FINANCIAL_DATASETS_API_KEY` in `.env` to use

The agent automatically selects the best available provider based on your configuration. By default, it uses free sources!

### LLM Configuration

Taleb works with **any OpenAI-compatible API**, giving you complete flexibility:

#### Option 1: OpenAI (Recommended for Production)
```bash
# In your .env file:
OPENAI_API_KEY=sk-...
# Optionally specify model:
# LLM_MODEL=gpt-4o
```

#### Option 2: OpenRouter (Access Multiple Models)
```bash
# In your .env file:
OPENAI_API_KEY=sk-or-v1-...
OPENAI_BASE_URL=https://openrouter.ai/api/v1
LLM_MODEL=anthropic/claude-3.5-sonnet
```

#### Option 3: Ollama (100% Free & Local)
```bash
# 1. Install Ollama from https://ollama.ai
# 2. Pull a model: ollama pull llama3.2
# 3. In your .env file:
OPENAI_BASE_URL=http://localhost:11434/v1
LLM_MODEL=llama3.2
OPENAI_API_KEY=not-needed
```

#### Option 4: llama.cpp (100% Free & Local)
```bash
# 1. Run llama.cpp server with --api-key option
# 2. In your .env file:
OPENAI_BASE_URL=http://localhost:8080/v1
LLM_MODEL=your-model-name
OPENAI_API_KEY=not-needed
```

#### Option 5: Other OpenAI-Compatible APIs
Works with sage.is, Together.ai, Anyscale, or any other OpenAI-compatible endpoint:
```bash
OPENAI_API_KEY=your-api-key
OPENAI_BASE_URL=https://your-provider.com/v1
LLM_MODEL=your-model-name
```

### Usage

Run Taleb in interactive mode:
```bash
uv run taleb-agent
```

### Example Queries

Try asking Taleb questions like:
- "What was Apple's revenue growth over the last 4 quarters?"
- "Compare Microsoft and Google's operating margins for 2023"
- "Analyze Tesla's cash flow trends over the past year"
- "What is Amazon's debt-to-equity ratio based on recent financials?"

Taleb will automatically:
1. Break down your question into research tasks
2. Fetch the necessary financial data
3. Perform calculations and analysis
4. Provide a comprehensive, data-rich answer

## Architecture

Taleb uses a multi-agent architecture with specialized components:

- **Planning Agent**: Analyzes queries and creates structured task lists
- **Action Agent**: Selects appropriate tools and executes research steps
- **Validation Agent**: Verifies task completion and data sufficiency
- **Answer Agent**: Synthesizes findings into comprehensive responses

## Project Structure

```
taleb/
├── src/
│   ├── taleb/
│   │   ├── agent.py              # Main agent orchestration logic
│   │   ├── model.py              # LLM interface
│   │   ├── prompts.py            # System prompts for each component
│   │   ├── schemas.py            # Pydantic models
│   │   ├── tools/
│   │   │   ├── finance/          # Financial data tools
│   │   │   │   ├── providers/    # Pluggable data providers
│   │   │   │   │   ├── base.py              # Abstract provider interface
│   │   │   │   │   ├── yahoo_finance.py     # Free Yahoo Finance provider
│   │   │   │   │   ├── sec_edgar.py         # Free SEC EDGAR provider
│   │   │   │   │   ├── financial_datasets.py # Paid provider (optional)
│   │   │   │   │   └── config.py            # Provider factory
│   │   │   │   ├── fundamentals.py
│   │   │   │   ├── prices.py
│   │   │   │   ├── news.py
│   │   │   │   └── ...
│   │   ├── utils/                # Utility functions
│   │   └── cli.py                # CLI entry point
├── pyproject.toml
└── uv.lock
```

## Configuration

Taleb supports configuration via the `Agent` class initialization:

```python
from taleb.agent import Agent

agent = Agent(
    max_steps=20,              # Global safety limit
    max_steps_per_task=5       # Per-task iteration limit
)
```

### Customizing Data Providers

You can also customize which data provider to use:

```python
from taleb.tools.finance.providers.config import ProviderFactory

# Use Yahoo Finance (free, default)
provider = ProviderFactory.create_provider("yahoo")

# Use SEC EDGAR (free, filings only)
provider = ProviderFactory.create_provider("sec")

# Use Financial Datasets (paid, requires API key)
provider = ProviderFactory.create_provider("financial_datasets", api_key="your-key")

# Use composite provider (combines Yahoo Finance + SEC EDGAR)
provider = ProviderFactory.get_composite_provider()
```

### Adding Your Own Data Provider

To integrate a trading API or another data source, create a new provider:

```python
from taleb.tools.finance.providers.base import DataProvider
from typing import Literal, Optional

class YourTradingAPIProvider(DataProvider):
    def __init__(this, api_key: str, **kwargs):
        super().__init__(api_key=api_key, **kwargs)
        # Initialize your API client
    
    def get_price_snapshot(this, ticker: str) -> dict:
        # Implement using your trading API
        pass
    
    def get_income_statements(this, ticker: str, period: Literal["annual", "quarterly", "ttm"], limit: int = 10, **kwargs) -> dict:
        # Implement using your trading API
        pass
    
    # Implement other required methods...
```

## How to Contribute

1. Fork the repository
2. Create a feature branch
3. Commit your changes
4. Push to the branch
5. Create a Pull Request

**Important**: Please keep your pull requests small and focused.  This will make it easier to review and merge.


## License

This project is licensed under the AGPL License.

