### Stock Movement Explanation Agent

## **Introduction**

This project implements a fully autonomous, explainable, and multi-turn conversational LLM-powered financial analysis system capable of explaining why a stock’s price moved during a specified time range. Built using Google’s next-generation Agents framework, the system demonstrates advanced AI Agent concepts, including multi-agent coordination, persistent memory, tool-driven data retrieval, context-aware parsing, and sequential execution pipelines.

The core motivation behind the project is that traditional stock movement explanations often require manually gathering data, interpreting charts, correlating sector trends, and reviewing financial statements. This system automates all of those steps by decomposing the problem across specialized agents—each with its own responsibilities—and orchestrating them into a coherent final answer.

The result is a reliable, explainable, and extensible AI system capable of finance research, price movement attribution, and multi-step reasoning across structured and unstructured data sources.


## **Project Objective**

The goal is to create an AI that can answer questions like:

“Explain MSFT from 2024-01-01 to 2024-02-01.”

followed by:

“How about this date?”
“What about NVDA last month?”

The system automatically:

- Understands tickers + dates from user language
- Resolves missing information using conversational memory
- Retrieves and analyzes stock data
- Gathers technical indicators
- Benchmarks the broader market
- Reads quarterly financial statements
- Synthesizes everything into a natural-language explanation

This creates a truly interactive, intelligent financial analysis assistant.

## **System Architecture Overview**
1. ParserAgent (LLM Agent)

Extracts ticker, start_date, and end_date from free-form user input.

Handles ambiguous follow-up queries (e.g., “How about this date?”).

Outputs structured JSON that downstream agents use.


2. Price Agent (LLM + yfinance Tool)

Retrieves historical OHLCV data and computes:

- Return
- Percentage change
- Price trajectory
- Number of trading days

3. Technical Agent (LLM + Python Tool)

Computes:

- Volatility
- Largest single-day move
- General trend classification
- Summary of technical patterns

4. Market Agent

Benchmarks the stock against major indices (e.g., SPY, QQQ) to contextualize sector momentum.

5. Financials Agent

Retrieves quarterly income statements using yfinance and highlights:

- Revenue trends
- Net income
- Profitability changes
- Events in quarterly filings that may explain price movement

6. OrchestratorAgent

Combines all outputs into a single, coherent explanation containing:

- Price trend summary
- Technical indicators
- Sector/market context
- Financial fundamentals

A final natural-language explanation of why movement occurred

## **Persistent Memory with InMemoryRunner**

A key advancement in this system is the use of InMemoryRunner instead of manually managing state or relying on the runner alone.

InMemoryRunner enables:

- Persistent per-user memory
- Multi-turn conversation flows
- State updates after each run
- Context consistency (ticker, dates, last query)
- Natural conversational experiences similar to ChatGPT

Example:

User:

“Explain MSFT from Jan 1 to Feb 1.”

Follow-up:

“How about this date?”

Resolver + Service memory infer:

- Same ticker (MSFT)
- Last end date → used as new start date
- New provided date → used as new end date

This produces smooth, natural dialog without requiring users to repeat context.

## **Agent Execution and Debugging**

The system uses:

- Runner.run_async() for event streaming
- Event objects to extract intermediate outputs
- Debug traces for full transparency
- Sequential execution to guarantee deterministic ordering

Every agent’s output is printed as the pipeline progresses, giving users observability into:

- Which agent is speaking
- What tools were invoked
- How data was transformed
- How the final reasoning is constructed

## **Tools & Integrations**
Custom Tools

- fetch_prices_yf
- analyze_technicals
- summarize_market_context
- fetch_quarterly_income_yf

Built-in Tools
- Gemini model execution
- Event streaming
- Debug framework

External Libraries

- yfinance
- pandas
- numpy

These tools give the agents structured, grounded evidence to reduce hallucinations and improve financial accuracy.

## **Why Sequential Agents?**

The problem of “explain price movement” is inherently sequential:

1. Get data →
2. Analyze →
3. Provide technical context →
4. Provide market context →
5. Provide financial context →
6. Synthesize

Each agent depends on the previous one’s output.
SequentialAgent enforces discipline and predictability across steps.

## **Agent Concepts Demonstrated**

This project demonstrates at least 8 of the required course concepts:

✔ 1. Multi-Agent System

Parser → Price → Technical → Market → Financials → Orchestrator

✔ 2. Sequential Agents

Deterministic reasoning chain.

✔ 3. LLM Agents

Every agent uses Gemini 2.0 Flash.

✔ 4. Custom Tools

Real-time data from yfinance.

✔ 5. InMemoryService (Sessions & Memory)

Persistent, multi-turn conversational memory.

✔ 6. Context Engineering

Parser + resolver pipeline avoids hallucinations and missing context.

✔ 7. Observability (Debug Events)

Prints each agent’s output.

✔ 8. Agent Evaluation (Qualitative)

Cross-validated by market data and financial statements.

## **Conclusion**

This project results in an advanced, end-to-end intelligent system capable of financial reasoning, multi-turn dialogue, contextual memory handling, and real-time market computations.

The architecture is modular, extensible, and production-ready, making it a strong template for future:

- Research assistants
- Financial chatbots
- AI-powered stock analysis tools
- Autonomous investment research systems

It successfully demonstrates high-level AI engineering concepts, agent orchestration, custom data tools, and persistent memory—all working together to form a powerful, coherent multi-agent solution.
