# 🔍 Automated AI Research Agent (n8n Sub-Workflow)

![n8n](https://img.shields.io/badge/n8n-Workflow-FF6C37?style=for-the-badge&logo=n8n)
![OpenAI](https://img.shields.io/badge/OpenAI-GPT--4o--mini-412991?style=for-the-badge&logo=openai)
![Google Search](https://img.shields.io/badge/Google-Custom_Search-4285F4?style=for-the-badge&logo=google)

This repository contains a robust n8n sub-workflow designed to perform automated, context-aware web research. Triggered by a parent workflow, it dynamically generates search terms, executes searches via the Google Custom Search API, scrapes the results, and uses an LLM to evaluate, filter, and summarize the content.

## ✨ Features
* **Dynamic Search Query Generation:** Uses an LLM to analyze the provided context and creatively brainstorm diverse Google search terms.
* **Automated Web Scraping:** Pulls search results from the Google API, extracts HTML content from the links, converts it to Markdown, and uses custom JavaScript to strip out unnecessary images and links for clean processing.
* **Intelligent Relevance Filtering:** After summarizing the scraped content, a dedicated LLM chain evaluates whether the summary is actually relevant to the initial user context, dropping irrelevant or empty results.
* **Sub-Workflow Architecture:** Designed to be called by another n8n workflow, taking in arguments (`context`, `top_results`, and `num_search_terms`) and passing back an aggregated JSON array of verified research.

## 🏗️ Architecture & Workflow Nodes

| Component | Node Name | Function |
| :--- | :--- | :--- |
| ⚡ **Trigger** | `When Executed by Another Workflow` | Accepts inputs from a parent n8n automation. |
| ⚙️ **Config** | `configuration` | Sets up Google API keys and runtime variables. |
| 🧠 **LLM Gen** | `create search terms` | Prompts GPT-4o-mini to generate varied search queries. |
| 🌐 **API** | `search on Google` | Executes HTTP request to Google Custom Search API. |
| 🕸️ **Scraper** | `get the content of the link` | Fetches the raw HTML of the search results. |
| 🧹 **Parser** | `HTML` -> `Markdown` -> `cleanup` | Converts body to text and strips junk elements. |
| 📝 **LLM Eval** | `summarize` & `is it relevant?`| Summarizes text and strictly filters for context fit. |
| 📦 **Output** | `set fields to return` -> `Aggregate` | Packages the final summaries and links for the parent workflow. |

## 🚀 Installation & Setup

1. **Prerequisites**: Ensure you have an active [Google Programmable Search Engine](https://programmablesearchengine.google.com/) configured, along with a Google Cloud API key.
2. **Import to n8n**: Open your n8n workspace, navigate to **Workflows > Import from File**, and upload the JSON file.
3. **Credentials Setup**:
   * Add your **OpenAI API Key** to the `OpenAI Chat Model` node (or swap it out for an Ollama node).
4. **Configuration**:
   * Open the `configuration` node (the Set node after the trigger).
   * Replace `YOUR_GOOGLE_API_KEY_COMES_HERE` with your actual Google API key.
   * Replace `YOUR_CUSTOM_SEARCH_ID_COMES_HERE` with your Google Custom Search Engine ID (CX).
5. **Usage**: Call this workflow from another n8n automation using the "Execute Workflow" node, passing `context` (string), `num_search_terms` (number), and `top_results` (number).

---
**Author:** Abu Huraira
