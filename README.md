# Financial Conversational Bot: Llama 3.1 Fine-Tuning Pipeline

This repository contains an end-to-end pipeline for building a finance-focused conversational AI. The project involves scraping real-time financial news from Indian markets, generating a synthetic instruction-tuning dataset, and fine-tuning a Llama 3.1 8B model using QLoRA.

## Pipeline Components

### 1. Data Scraping & Extraction
Located in the initial notebooks, this stage uses **Selenium** and **BeautifulSoup** to extract clean text data from:
* **The Economic Times:** Corporate trends, policy news, and stock updates.
* **Moneycontrol:** Company-specific news and market analysis.
* *Technical Note:* Implemented multi-processing to handle high-volume scraping and utilized Selenium's `eager` page load strategy to navigate dynamic "Load More" elements efficiently.

### 2. Synthetic QA Generation
Since raw news articles are not in a conversational format, I built a synthetic data pipeline:
* **Question Generation:** Uses `mrm8488/t5-base-finetuned-question-generation-ap` to turn news summaries into relevant questions.
* **Answer Validation:** Uses `distilbert-base-uncased-distilled-squad` to extract precise answers from the context.
* **Formatting:** The resulting pairs are formatted into the Llama 3.1 Chat Template (JSON).

### 3. Model Fine-Tuning (QLoRA)
The core of the project is the fine-tuning of **Llama-3.1-8B-Instruct**. 
* **Quantization:** 4-bit (NF4) to minimize VRAM usage.
* **PEFT Technique:** LoRA (Rank 64, Alpha 16) targeting all linear modules (q, k, v, o, gate, up, down projects).
* **Optimization:** Paged AdamW 32-bit with a constant learning rate scheduler.

## Performance & Results
After training for 3 epochs on the custom financial dataset:
* **Validation Loss:** 1.57
* **BERTScore (F1):** 0.85
* The model demonstrates a strong ability to recall specific financial facts and recommendations (e.g., brokerage ratings on stocks like Avanti Feeds) that were present in the scraped context.

## Disclaimer
This project is for educational purposes, and Financial data is scraped for personal use only.
