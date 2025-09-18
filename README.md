# 📜 LegalLens – Backend

Clarity Counsel AI is a backend service that analyzes **Indian legal documents** (PDF, DOCX, or plain text) and produces a **clause-by-clause JSON output** highlighting the severity of each clause:

* 🔴 **High Risk** → strong obligations, penalties, or one-sided terms
* 🟡 **Caution** → vague, unusual, or needs clarification
* 🟢 **Standard** → normal, fair, and expected

This helps everyday citizens and small businesses **understand risks in contracts** without legal jargon.

---

## 🚀 Features

* Accepts **PDF, DOCX, and plain text** inputs
* Segments documents into **individual clauses**
* Uses **Google Cloud Vertex AI (Gemini)** for risk classification
* Returns a **structured JSON** response for easy integration with other systems

---

## 📂 Project Structure

```
clarity_counsel_backend/
│── requirements.txt
│── main.py
│
├── src/
│   ├── ingest.py   
│   ├── segmenter.py 
│   ├── classifier.py 
│   └── pipeline.py 
│
├── data/
│   ├── samples/ 
│   └── output/  
```

---

## ⚙️ Installation & Setup

### 1. Clone the Repository

```bash
git clone https://github.com/your-username/clarity_counsel_backend.git
cd clarity_counsel_backend
```

### 2. Install Dependencies

```bash
pip install -r requirements.txt
```

### 3. Authenticate with Google Cloud

Make sure you have Vertex AI enabled in your Google Cloud project:

```bash
gcloud auth application-default login
```

### 4. Run the Backend

```bash
uvicorn main:app --reload
```

Backend will be available at:
👉 `http://127.0.0.1:8000`

---

## 📡 API Usage

### Endpoint: `/analyze`

**POST** request with either:

* a file (`pdf`, `docx`, `txt`)
* or raw text

#### Example with cURL (file upload)

```bash
curl -X POST "http://127.0.0.1:8000/analyze" \
     -F "file=@data/samples/rental_agreement.pdf"
```

#### Example with cURL (pasted text)

```bash
curl -X POST "http://127.0.0.1:8000/analyze" \
     -F "text=The tenant must pay rent by the 5th of each month..."
```

---

## 📊 Sample JSON Output

```json
{
  "document": "rental_agreement.pdf",
  "analysis": [
    {"clause": "Tenant must pay rent by the 5th of each month", "severity": "🟢"},
    {"clause": "If rent is delayed, penalty of 10% per day applies", "severity": "🔴"},
    {"clause": "Landlord may terminate without notice in case of dispute", "severity": "🔴"},
    {"clause": "Tenant responsible for minor repairs", "severity": "🟡"}
  ]
}
```