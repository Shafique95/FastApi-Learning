

## ✅ FastAPI + venv প্রাথমিক সেটআপ (Step-by-Step)

### 🔹 ১. প্রজেক্ট ফোল্ডার তৈরি করুন

```bash
mkdir fastapi-app
cd fastapi-app
```

---

### 🔹 ২. ভার্চুয়াল এনভায়রনমেন্ট তৈরি ও অ্যাক্টিভ করুন

```bash
python3 -m venv venv
source venv/bin/activate
```

---

### 🔹 ৩. FastAPI ও Uvicorn ইনস্টল করুন

```bash
pip install fastapi uvicorn[standard]
```

---

### 🔹 ৪. একটি মিনিমাল FastAPI অ্যাপ তৈরি করুন

**ফাইল:** `main.py`

```python
from fastapi import FastAPI

app = FastAPI()

@app.get("/")
async def root():
    return {"message": "Hello, FastAPI!"}
```

---

### 🔹 ৫. লোকাল সার্ভার চালু করুন

```bash
uvicorn main:app --reload
```

এখন ব্রাউজারে যান: 👉 `http://127.0.0.1:8000`
**API running**
Docs: 👉 `http://127.0.0.1:8000/docs`

---

### 🔹 ৬. আপনার ডিপেন্ডেন্সি লিস্ট করুন (পরবর্তীতে কাজে লাগবে)

```bash
pip freeze > requirements.txt
```

---

## ✅ স্ট্রাকচার হবে এমন:

```
fastapi-app/
│
├── main.py
├── venv/               # virtual environment (ignore in git)
├── requirements.txt
```

---

## ✳️ পরবর্তী ধাপ?

* চাইলে আমি ডেটাবেইজ (PostgreSQL/SQLite) যুক্ত করে দিতে পারি
* Docker সেটআপ করতে পারি
* অথবা প্রোডাকশন গাইড দিতে পারি (Gunicorn + Nginx + Supervisor)

---

আপনি শুধু বলুন:
👉 আপনি কোন দিক থেকে শুরু করতে চান?

* শুধু API?
* ডেটাবেইজ সহ?
* Docker সহ?
* Deploy শিখতে চান?

আমি পুরো গাইড সাজিয়ে দিচ্ছি।
