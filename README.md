<div align="center">

# 💹 Financial Conversational Bot  
## Llama 3.1 Fine-Tuning Pipeline (QLoRA)

[![Python](https://img.shields.io/badge/Python-3.10+-blue.svg)](https://www.python.org/)
[![LLM](https://img.shields.io/badge/LLM-Llama%203.1%208B%20Instruct-purple.svg)]()
[![Fine-Tuning](https://img.shields.io/badge/Fine--Tuning-QLoRA%20%7C%20LoRA-green.svg)]()
[![Web Scraping](https://img.shields.io/badge/Web%20Scraping-Selenium%20%7C%20BeautifulSoup-orange.svg)]()

🔗 **GitHub Repo:** https://github.com/sumits234/Fine-tuning-a-LLM-to-Create-a-Finance-Domain-chatbot

</div>

---

## 🚀 Project Overview
This repository contains an **end-to-end pipeline** for building a **finance-focused conversational AI**.  
The system scrapes real-time market news (India), generates a synthetic instruction-tuning dataset, and fine-tunes a **Llama 3.1 8B Instruct** model using **QLoRA**.

✅ End Goal: a chatbot that can answer finance questions **contextually** using scraped market information.

---

## 🧠 Pipeline Components

### 1) Data Scraping & Extraction
This stage extracts clean finance text from Indian market news sources using **Selenium** and **BeautifulSoup**:

- **The Economic Times** → corporate trends, policy news, stock updates  
- **Moneycontrol** → company-specific news, market analysis  

**Technical Highlights**
- Implemented **multi-processing scraping** to handle high-volume pages
- Used Selenium **`eager` page load strategy**
- Automated dynamic navigation such as **"Load More"** interactions

---

### 2) Synthetic Instruction Dataset Generation (QA)
Since raw market news is not conversational, the pipeline generates synthetic Q&A pairs:

- **Question Generation (T5):** `mrm8488/t5-base-finetuned-question-generation-ap`
- **Answer Extraction / Validation (BERT):** `distilbert-base-uncased-distilled-squad`
- **Formatting:** Converted to **Llama 3.1 Chat Template** (JSON format)

✅ Output: high-quality instruction-tuning samples (question + answer) grounded in scraped context.

---

### 3) Fine-Tuning Llama 3.1 8B (QLoRA)
Fine-tuned **Llama-3.1-8B-Instruct** with QLoRA to reduce GPU memory requirements.

**Training Setup**
- **Quantization:** 4-bit NF4
- **PEFT:** LoRA *(Rank=64, Alpha=16)*
- **Target modules:** all linear modules  
  *(q, k, v, o, gate, up, down projections)*
- **Optimizer:** Paged AdamW 32-bit
- **LR Scheduler:** constant

---

## 📊 Performance & Results
After training for **3 epochs** on the finance instruction dataset:

- **Validation Loss:** `1.57`
- **BERTScore (F1):** `0.85`

✅ The chatbot demonstrates strong ability to recall and answer finance-related questions using scraped context  
(e.g., brokerage rating / stock recommendations like **Avanti Feeds**).

---

## 🛠️ Tech Stack
- Python
- Selenium + BeautifulSoup
- HuggingFace Transformers
- PEFT (LoRA/QLoRA)
- BitsAndBytes (4-bit quantization)
- PyTorch
- BERTScore for evaluation


## Disclaimer
This project is for educational purposes only. All financial data is scraped strictly for personal use and research. This repository does not provide financial advice.

