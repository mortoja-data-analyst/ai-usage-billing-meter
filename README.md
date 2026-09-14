# 🚀 Enterprise AI Usage-Based Billing Meter

A production-grade, highly scalable telemetry system built with **FastAPI** to track AI model token usage via **Tiktoken** and dynamically report metered billing events directly to **Stripe Billing**.

---

## ✨ Features
* **Accurate Token Metering:** Utilizes OpenAI's official `tiktoken` library to calculate exact prompt token lengths.
* **Asynchronous Non-Blocking Workers:** Leverages FastAPI's `BackgroundTasks` to send telemetry to Stripe asynchronously, ensuring zero impact on API response latency.
* **Modular Clean Architecture:** Properly segregated modules for `TokenCalculator`, `StripeBillingManager`, and the main API routing.
* **Robust Error Handling:** Comprehensive logging for tokenization anomalies and Stripe communication failures.
* **Security First:** Zero hardcoded secrets; fully compatible with `.env` files for environment variable configuration.

---

## 📂 Project Architecture & Structure

```text
ai-usage-billing-meter/
│
├── app/
│   ├── __init__.py          # Package initialization
│   ├── main.py              # Central FastAPI app & API endpoints
│   ├── tracker.py           # Tiktoken-based token counting engine
│   └── stripe_helper.py     # Stripe Billing integration wrapper
│
├── .env.example             # Configuration template for secrets
├── .gitignore               # Environment and cache protection file
└── README.md                # System documentation (This file)
```

---

## 🛠️ Setup & Installation Instructions

Follow these quick steps to deploy and test the project locally:

### 1. Clone the Repository
```bash
git clone <your-github-repo-url>
cd ai-usage-billing-meter
```

### 2. Install Dependencies
Install all the required standard production packages:
```bash
pip install fastapi uvicorn stripe tiktoken python-dotenv httpx
```

### 3. Configure Credentials
Copy the environment template and insert your actual production or test Stripe Secret Key:
```bash
cp .env.example .env
```
Open the newly created `.env` file and replace the placeholder:
```ini
STRIPE_API_KEY=sk_test_your_real_stripe_secret_key
```

### 4. Run the Production Server
Start the Uvicorn server locally on port 8000:
```bash
uvicorn app.main:app --host 127.0.0.1 --port 8000 --reload
```

---

## 🧪 API Endpoint Verification

You can instantly test the token tracking pipeline by executing a POST request to the application's telemetry endpoint:

* **Endpoint:** `POST http://127.0.0`
* **Content-Type:** `application/json`

### Sample Request Body (Payload)
```json
{
  "user_id": "user_production_99",
  "stripe_customer_id": "cus_R3alStripeCustID",
  "prompt": "Hello Python, please calculate this prompt and log the usage telemetry directly to my Stripe customer account."
}
```

### Expected JSON Response
```json
{
  "status": "telemetry_logged",
  "tokens_measured": 20
}
```
*Note: The actual token number will automatically vary based on the prompt's token length computed by the tokenizer.*
