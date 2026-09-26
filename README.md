# Zepto Data & AI Platform - Capstone Project

This repository contains a connected AI/ML platform for Zepto, consisting of three integrated modules: a raw-to-relational data pipeline, an end-to-end analytics and predictive modeling pipeline, and a grounded GenAI support assistant.

## 📁 Project Structure

```text
.
├── data_pipeline/        # Module 1: Scraping & Relational Storage
├── analytics/            # Module 2: EDA & Predictive Modeling
└── support_assistant/    # Module 3: Grounded GenAI Service
```

---

## 🚀 Module 1: Data Pipeline (`/data_pipeline`)
**Goal**: Scrape product data from a public site and load it into a normalized SQLite database.

### Setup & Execution
1. Navigate to the folder: `cd data_pipeline`
2. Install dependencies: `pip install requests beautifulsoup4 pandas`
3. Run the pipeline: Open and execute `BookVault_Data_Analytics.ipynb` from top to bottom.

### Design Summary
The pipeline scrapes 85 books across 4 categories from `books.toscrape.com`. It implements a robust cleaning stage using regex for price extraction and median imputation for missing values. The data is stored in a normalized SQLite schema with a `categories` table and a `books` table linked by a foreign key. The implementation is verified by comparing SQL JOIN results with Pandas `merge` operations.

---

## 📊 Module 2: Analytics Pipeline (`/analytics`)
**Goal**: Profile the Titanic dataset and build a predictive survival model.

### Setup & Execution
1. Navigate to the folder: `cd analytics`
2. Install dependencies: `pip install pandas seaborn scikit-learn imbalanced-learn joblib`
3. Run the analysis: Open and execute `Titanic_Survival_Prediction_Model.ipynb`.

### Design Summary
The pipeline follows a rigorous data science workflow: profiling missing values $\rightarrow$ IQR-based outlier detection $\rightarrow$ stratified splitting $\rightarrow$ leakage-free preprocessing. I compared three classifiers (Logistic Regression, Decision Tree, Random Forest) and used SMOTE to handle class imbalance. The final model is exported as a complete `scikit-learn` Pipeline (`.joblib`) including the fitted scaler and encoder for end-to-end raw data inference.

---

## 🤖 Module 3: Support Assistant (`/support_assistant`)
**Goal**: A grounded RAG service to answer policy questions using LangGraph and FastAPI.

### Setup & Execution
1. Navigate to the folder: `cd support_assistant`
2. Install dependencies: `pip install -r requirements.txt`
3. Start the server: `python main.py`
4. Test the API: Use the `curl` commands provided in `support_assistant/README.md`.

### Design Summary
The assistant uses a RAG (Retrieval-Augmented Generation) architecture. It employs `all-MiniLM-L6-v2` for embeddings and ChromaDB for cosine similarity retrieval. A LangGraph `StateGraph` orchestrates the flow, routing queries between a policy retrieval node and a general answer node. The system enforces a strict JSON schema via Pydantic and includes a simulated retry loop for the optional `MOCK_LLM=0` path to demonstrate robust error handling.

---

## 🛠️ General Installation
If you prefer to install all dependencies at once, you can run the following:
```bash
pip install requests beautifulsoup4 pandas seaborn scikit-learn imbalanced-learn joblib fastapi uvicorn chromadb langgraph sentence-transformers pydantic
```

## 📈 Git Workflow
The project follows a professional Git workflow. All major features were developed on dedicated feature branches, tested, and integrated into the `main` branch via Pull Requests to ensure code stability and a clear audit trail of changes.
