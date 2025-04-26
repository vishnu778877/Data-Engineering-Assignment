# Data Engineering Pipeline for Logistics & Supply Chain Data

This project builds a scalable, automated pipeline to crawl logistics and supply chain websites, fetch logistics-related news articles, and store both raw and processed data into cloud storage.  
Due to credit card limitations, **Backblaze B2** (an S3-compatible service) was used instead of AWS S3 for cloud storage.

The system ensures modular, cloud-scalable crawling and structured storage for easy future analysis and processing.

---

## Features:
- Crawls both **static** and **dynamic** logistics websites using Python.
- Fetches logistics news articles through free **RSS feeds**.
- Stores **raw HTML** and **processed JSON** data into **Backblaze B2** cloud.
- Organizes cloud storage using a **partitioned folder structure** by source and date.
- Implements basic **error handling** for failed crawls and uploads.

---

## 1. Code Overview:

### 1.1 Cloud Storage Setup:
- Connects to **Backblaze B2** using `boto3`.
- Configures the **Access Key**, **Secret Key**, and **Endpoint URL** for cloud uploads.
- Data is partitioned by `source_name/YYYY-MM-DD/` folders inside the cloud bucket.

---

### 1.2 Web Crawling:
- Static websites like Freightwaves, MarketsandMarkets, GnosisFreight are crawled using `requests` and `BeautifulSoup`.
- Dynamic websites like Leadiq and G2 are crawled using `Playwright` to handle JavaScript-rendered pages.
- Each website has a **separate modular crawler function** for easier maintenance.

---

### 1.3 News API Integration:
- Logistics news is fetched using **RSS feeds** with the `feedparser` library.
- Each fetched article is stored as a **separate JSON file** under the source’s date folder.

---

### 1.4 Upload Functions:
- Uploads raw HTML pages and JSON articles into Backblaze B2 storage.
- Ensures organized storage structure for easy querying and data management.

---

### 1.5 Error Handling:
- Includes basic error handling with try-except blocks.
- Logs errors during crawling or cloud upload failures for review and debugging.

---

## 2. How to Run the Code:

1. Install the required Python libraries:
   ```bash
   pip install requests beautifulsoup4 boto3 feedparser playwright
   playwright install
   ```

2. Update the cloud credentials section in the notebook with:
   - **Access Key ID**
   - **Secret Access Key**
   - **Endpoint URL**
   - **Bucket Name**

3. Run each crawler function sequentially in the Google Colab notebook.

4. Crawled web pages and news articles will be automatically uploaded to your Backblaze B2 bucket.

---

## 3. Folder Structure in Cloud Storage:

```
logistics-data/
├── freightwaves/
│   └── YYYY-MM-DD/
│       └── page.html
├── freightwaves-news/
│   └── YYYY-MM-DD/
│       ├── article1.json
│       ├── article2.json
├── gnosisfreight/
│   └── YYYY-MM-DD/
│       └── page.html
├── marketsandmarkets/
│   └── YYYY-MM-DD/
│       └── page.html
└── ...
```

---

## 4. Output:

After running the code, you will have:

- Crawled logistics websites stored as raw HTML in cloud.
- Fetched logistics news articles saved as individual JSON files.
- A cleanly organized cloud storage partitioned by source and date.

---

## 5. Improvements (Future Work):

- Adding scheduled crawls (daily/hourly) using `schedule` library or GitHub Actions.
- Implementing detailed logging and retry mechanisms for crawlers.
- Deduplicating articles based on URL hashes to avoid duplicates.
- Running basic text processing (NER, keyword extraction) on crawled articles.
- Dockerizing the full pipeline for easy deployment and portability
