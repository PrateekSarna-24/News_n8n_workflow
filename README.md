# Automated News Search & Social Media Posting Workflow

This document explains the **automated workflow** I designed in **n8n** to fetch daily news, summarize them using AI, and publish directly to social media platforms. The system runs automatically every morning, while also allowing manual execution.

---

## 1. Workflow Triggers

* **Manual Trigger** → Allows running the workflow on demand.
* **Scheduled Trigger (Cron)** → Runs automatically **every day at 9 AM** to fetch and process the latest news.

---

## 2. Fetching News Articles

Initially, we attempted to use the **Google News API**, but encountered issues with **redirection errors**. To solve this, we switched to the **GNews API**, which provided reliable access to current news.

* Three separate **HTTP Request nodes** were set up, each with different query parameters to ensure we retrieved at least 3 articles per run.

---

## 3. Data Aggregation

All three HTTP requests feed into a **JavaScript Code Node**.

* This node extracts important JSON fields such as:

  * **Title**
  * **Image Link**
  * **Article Link**
  * **Metadata** (author, date, etc.)
* The extracted information is then forwarded to three different downstream nodes.

---

## 4. Processing Pipeline

The aggregated JSON is pushed into three parallel nodes:

1. **HTTP Request Node (Article Fetching)** → Retrieves the full article text.
2. **HTTP Request Node (Image Fetching)** → Retrieves the article image.
3. **JavaScript Code Node** → Forwards the metadata (title, link, etc.).

---

## 5. Text Extraction & Summarization

The **article fetching node** is connected to:

* **Text Extractor Node** → Cleans and extracts raw article text.
* **Together AI Summarizer (HTTP Request Node)** →

  * Uses my **Together API Key** to make a `POST` request.
  * Sends the extracted text to the AI model.
  * Returns a **concise AI-generated summary**.

---

## 6. File Conversion

The final outputs — **summarized text**, **article image**, and **metadata** — are connected to a **File Conversion Node**:

* Converts the structured data into a **JSON file output**.

---

## 7. Social Media Publishing

The **JSON output** is connected to a **Social Media HTTP Request Node**:

* This node makes an API call to the chosen **social media platform**.
* Posts the article summary along with the **image** and **link**.

---

## ✅ Summary of Workflow

1. Trigger (manual or scheduled).
2. Fetch 3+ news articles using **GNews API**.
3. Extract important data via a **JavaScript node**.
4. Parallel processing for article, image, and metadata.
5. Summarize text using **Together AI**.
6. Convert results into a **JSON file**.
7. Publish directly to **social media**.

---

### Benefits for Employer

* **Fully automated**: Runs daily at 9 AM.
* **Scalable**: Can easily increase number of articles or platforms.
* **Flexible**: Switch between APIs when one fails (e.g., Google News → GNews).
* **AI-powered summaries**: Provides clear, concise content ready for posting.
* **Multi-platform support**: Works with any social media via API integration.

---
