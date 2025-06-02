```python
# Dependency list
api_dependencies = [
    Depends(is_valid_request_from_client),
    Depends(server_authorization)
]

# Protected router
protected_router = APIRouter(
    prefix="/blogs",
    tags=["Blog (Protected)"],
    dependencies=api_dependencies
)
```
দারুণ! আপনি জানতে চাচ্ছেন নিচের কোড ব্লকের **“behind-the-scenes” flow** বা **ভিতরের কাজ কীভাবে হয়**, সেটি **ধাপে ধাপে ব্যাখ্যা করছি** বাংলা ভাষায়:

---

## ✅ কোডটি:

```python
# Dependency list
api_dependencies = [
    Depends(is_valid_request_from_client),
    Depends(server_authorization)
]

# Protected router
protected_router = APIRouter(
    prefix="/blogs",
    tags=["Blog (Protected)"],
    dependencies=api_dependencies
)
```

---

## 🔄 এই কোডের কাজের ধাপ (Flow):

### 🧩 Concept:

FastAPI-এর `dependencies=[]` লিস্টে আপনি যেসব `Depends(...)` দেন, সেগুলো **প্রত্যেক রিকোয়েস্টেই** রান হয় **যেকোনো route কল হবার আগেই**।

---

## 🔍 ধাপে ধাপে ব্যাখ্যা:

### ✅ Step 1: Client Request পাঠায়

```http
GET /blogs/anything
Authorization: Bearer <token>
X-Custom-Header: abc123
```

---

### ✅ Step 2: FastAPI Router দেখবে `dependencies=[]`

রাউটারটি রিকোয়েস্ট আসার সাথে সাথেই নিচের দুটি ফাংশন sequentially execute করবে:

1. `is_valid_request_from_client`
2. `server_authorization`

---

### ✅ Step 3: FastAPI চালাবে `is_valid_request_from_client`

```python
def is_valid_request_from_client(request: Request):
    if request.headers.get("X-Custom-Header") != "abc123":
        raise HTTPException(status_code=400, detail="Invalid client")
```

🔹 যদি হেডার মেলে না, তাহলে **`400 Bad Request`** দিয়ে রিকোয়েস্ট আটকে যাবে।

---

### ✅ Step 4: এরপর চালাবে `server_authorization`

```python
def server_authorization(request: Request):
    auth_header = request.headers.get("Authorization")
    if not auth_header or not verify_token(auth_header):
        raise HTTPException(status_code=401, detail="Unauthorized access")
```

🔹 যদি টোকেন সঠিক না হয় → **`401 Unauthorized`**

---

### ✅ Step 5: যদি দুইটিই ঠিক থাকে ➤ **রুট ফাংশন রান হবে**

```python
@protected_router.get("/secure-data")
def get_data():
    return {"data": "protected info"}
```

এখন রিকোয়েস্টটি `get_data()` ফাংশনে যাবে এবং রেসপন্স আসবে।

---

## 🔁 সংক্ষেপে সম্পূর্ণ flow:

```text
Client → /blogs/secure-data
    ↓
FastAPI → Executes: 
    1. is_valid_request_from_client ✅
    2. server_authorization ✅
    ↓
If all OK → Runs actual endpoint function → Returns response ✅
```

---

## 📌 Bonus Tips:

| ব্যাপার                     | বিস্তারিত                                                 |
| --------------------------- | --------------------------------------------------------- |
| `dependencies=[]`           | শুধু side-effect/fail চেক করে, রিটার্ন ভ্যালু রুটে যায় না |
| যদি রিটার্ন ভ্যালু দরকার হয় | তাহলে ফাংশনেই `Depends(...)` ব্যবহার করতে হবে             |
| Error হলে                   | FastAPI সেই মুহূর্তেই HTTPException দিয়ে থেমে যাবে        |

---

## 🧠 আপনি যদি চান:

আমি `is_valid_request_from_client` এবং `server_authorization` সহ একটি **সম্পূর্ণ চলমান বাস্তব উদাহরণ** তৈরি করে দিতে পারি – যেখানে টোকেন, হেডার চেকসহ API কাজ করে।
জানান, আমি সেটা ১০ মিনিটের মধ্যে লিখে দিচ্ছি ✅
