Here is a **FastAPI Handbook (concise + practical + cheat sheet style)** you can use as a quick reference and learning guide.

---

# 🚀 FASTAPI HANDBOOK (CHEAT SHEET + GUIDE)

---

## 1. 🔹 What is FastAPI?

* A modern Python web framework for building APIs
* Based on:

  * **Starlette** (web handling)
  * **Pydantic** (data validation)
* Key benefits:

  * Fast (async support)
  * Automatic docs
  * Type-safe

---

## 2. 🔹 Installation

```bash
pip install fastapi uvicorn
```

Run server:

```bash
uvicorn main:app --reload
```

---

## 3. 🔹 Basic App

```python
from fastapi import FastAPI

app = FastAPI()

@app.get("/")
def read_root():
    return {"message": "Hello World"}
```

---

## 4. 🔹 Path Parameters

```python
@app.get("/items/{item_id}")
def get_item(item_id: int):
    return {"item_id": item_id}
```

✔ Type validation automatically happens

---

## 5. 🔹 Query Parameters

```python
@app.get("/items/")
def get_items(limit: int = 10):
    return {"limit": limit}
```

---

## 6. 🔹 Request Body (Pydantic Model)

```python
from pydantic import BaseModel

class Item(BaseModel):
    name: str
    price: float
    is_offer: bool = False

@app.post("/items/")
def create_item(item: Item):
    return item
```

✔ Auto validation + parsing

---

## 7. 🔹 Response Model

```python
@app.get("/items/", response_model=Item)
def get_item():
    return {"name": "Laptop", "price": 50000}
```

✔ Ensures clean output

---

## 8. 🔹 HTTP Methods

| Method | Use       |
| ------ | --------- |
| GET    | Read data |
| POST   | Create    |
| PUT    | Update    |
| DELETE | Remove    |

---

## 9. 🔹 Status Codes

```python
from fastapi import status

@app.post("/items/", status_code=status.HTTP_201_CREATED)
```

---

## 10. 🔹 Dependency Injection

```python
from fastapi import Depends

def common_params(q: str = None):
    return q

@app.get("/items/")
def read_items(q: str = Depends(common_params)):
    return {"q": q}
```

✔ Reusable logic

---

## 11. 🔹 Path vs Query

| Type  | Example           |
| ----- | ----------------- |
| Path  | `/items/1`        |
| Query | `/items?limit=10` |

---

## 12. 🔹 Validation Features

```python
from pydantic import BaseModel, Field

class Item(BaseModel):
    name: str = Field(min_length=3)
    price: float = Field(gt=0)
```

✔ Constraints:

* `gt` → greater than
* `lt` → less than

---

## 13. 🔹 Async Support

```python
@app.get("/")
async def async_func():
    return {"msg": "async response"}
```

✔ Use async for:

* DB calls
* APIs
* I/O tasks

---

## 14. 🔹 Middleware

```python
@app.middleware("http")
async def add_process_time(request, call_next):
    response = await call_next(request)
    return response
```

---

## 15. 🔹 CORS

```python
from fastapi.middleware.cors import CORSMiddleware

app.add_middleware(
    CORSMiddleware,
    allow_origins=["*"],
    allow_methods=["*"],
)
```

---

## 16. 🔹 Authentication (Basic Example)

```python
from fastapi import Header

@app.get("/secure")
def secure_api(token: str = Header()):
    return {"token": token}
```

---

## 17. 🔹 File Upload

```python
from fastapi import File, UploadFile

@app.post("/upload/")
def upload(file: UploadFile):
    return {"filename": file.filename}
```

---

## 18. 🔹 Background Tasks

```python
from fastapi import BackgroundTasks

def write_log():
    print("Log written")

@app.get("/")
def run_task(background_tasks: BackgroundTasks):
    background_tasks.add_task(write_log)
    return {"msg": "Task running"}
```

---

## 19. 🔹 Database (Simple Example)

Using SQLite + SQLAlchemy:

```python
from sqlalchemy import create_engine

engine = create_engine("sqlite:///test.db")
```

✔ Typically use:

* SQLAlchemy ORM
* Alembic (migrations)

---

## 20. 🔹 Project Structure (Recommended)

```
app/
 ├── main.py
 ├── models.py
 ├── schemas.py
 ├── database.py
 ├── routers/
 └── services/
```

---

## 21. 🔹 Routers (Modular Code)

```python
from fastapi import APIRouter

router = APIRouter()

@router.get("/")
def home():
    return {"msg": "router"}

app.include_router(router, prefix="/api")
```

---

## 22. 🔹 Automatic Docs

* Swagger UI → `/docs`
* ReDoc → `/redoc`

✔ Generated automatically

---

## 23. 🔹 Error Handling

```python
from fastapi import HTTPException

@app.get("/items/{id}")
def get_item(id: int):
    if id != 1:
        raise HTTPException(status_code=404, detail="Not found")
```

---

## 24. 🔹 FastAPI vs Flask (Quick)

| Feature    | FastAPI  | Flask   |
| ---------- | -------- | ------- |
| Speed      | High     | Medium  |
| Validation | Built-in | Manual  |
| Async      | Yes      | Limited |
| Docs       | Auto     | Manual  |

---

## 25. 🔹 Deployment

Run production:

```bash
uvicorn main:app --host 0.0.0.0 --port 8000
```

Common platforms:

* AWS EC2
* Docker
* Railway / Render

---

# ⚡ FINAL QUICK CHEAT SHEET

```python
from fastapi import FastAPI

app = FastAPI()

@app.get("/")
def home():
    return {"msg": "Hello"}

@app.post("/items/")
def create(item: dict):
    return item
```

---

# 📌 WHAT TO PRACTICE (IMPORTANT FOR YOU)

Since you're learning ML:

1. Build **Model API**
2. Deploy ML model using FastAPI
3. Add:

   * Input validation
   * Prediction endpoint
   * Logging
