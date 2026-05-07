# 🐳 Docker

---

# 📌 1. What is Docker?

Docker is a platform used to **develop, ship, and run applications inside containers**.

👉 It packages:

- Application code
- Libraries
- Dependencies
- Environment

➡️ Into one unit called a **container**

---

## 🎯 Problem Without Docker

- Works on developer system ✅
- Fails on another system ❌

### Reasons:

- Different OS
- Different software versions
- Missing dependencies

---

## ✅ Solution with Docker

👉 “Build once, run anywhere”

- Same environment everywhere
- No installation issues
- Easy deployment

---

# 📦 2. What is a Container?

A container is a **lightweight isolated environment** where an application runs.

### 🔹 Features:

- Fast startup
- Small size
- Efficient resource usage

---

## 🆚 Container vs Virtual Machine

| Feature | Container 🚀 | Virtual Machine 🐘 |
| --- | --- | --- |
| Size | Small | Large |
| Speed | Fast | Slow |
| OS | Shared | Separate OS |
| Performance | High | Medium |

---

# 🧱 3. Docker Architecture

- **Docker Client** → runs commands
- **Docker Daemon** → builds & runs containers
- **Docker Image** → blueprint
- **Docker Container** → running app

---

# 📝 4. Dockerfile

A Dockerfile is a **set of instructions to build a Docker image**

---

## 🔧 Example

```docker
FROM node:18

WORKDIR /app

COPY package*.json ./
RUN npm install

COPY . .

CMD ["npm", "start"]
```

---

## 🔍 Explanation

- `FROM` → base image
- `WORKDIR` → working directory
- `COPY` → copy files
- `RUN` → execute command
- `CMD` → run app

---

# 🔄 5. Docker Workflow

1. Write Dockerfile
2. Build Image
3. Run Container

---

## 🛠 Commands

```bash
docker build -t myapp .
docker run -p 3000:3000 myapp
```

---

# 🚀 6. Single-Stage Docker

## 📌 Definition

All steps (build + run) happen in one stage

---

## 📦 Example

```docker
FROM node:18

WORKDIR /app

COPY package*.json ./
RUN npm install

COPY . .

RUN npm run build

CMD ["npm", "start"]
```

---

## ❗ Disadvantages

- Large image size
- Contains unnecessary files
- Slower deployment
- Less secure

---

# 🚀 7. Multi-Stage Docker

## 📌 Definition

Uses multiple `FROM` statements to separate build and runtime

---

## 📦 Example

```docker
# Stage 1: Build
FROM node:18 AS builder

WORKDIR /app
COPY package*.json ./
RUN npm install

COPY . .
RUN npm run build

# Stage 2: Production
FROM nginx:alpine

COPY --from=builder /app/build /usr/share/nginx/html
```

---

## ✅ Advantages

- Smaller image
- Faster deployment
- Better security
- Clean production image

---

# ⚖️ 8. Single vs Multi-Stage

| Feature | Single Stage | Multi Stage |
| --- | --- | --- |
| Image Size | Large | Small |
| Speed | Slow | Fast |
| Security | Low | High |

---

# 🧾 9. Python Flask Application

Using Flask

---

## 📁 Project Structure

```
my-docker-app/
 ├── app.py
 ├── requirements.txt
 ├── Dockerfile.single
 └── Dockerfile.multi
```

---

## 🧠 app.py (Corrected)

```python
from flask import Flask

app = Flask(__name__)

@app.route("/")
def home():
    return "🚀 Hello from Docker App!"

@app.route("/about")
def about():
    return "This is running inside Docker!"

if __name__ == "__main__":
    app.run(host="0.0.0.0", port=5000)
```

---

## 📦 requirements.txt

```
Flask==3.0.0
```

---

# 🚀 10. Single-Stage Dockerfile

```docker
FROM python:3.10

WORKDIR /app

COPY requirements.txt .
RUN pip install -r requirements.txt

COPY . .

CMD ["python", "app.py"]
```

---

## ▶️ Run Commands (Corrected)

```bash
docker build -f Dockerfile.single -t flask-single .
docker run -p 5000:5000 flask-single
```

---

# 🚀 11. Multi-Stage Dockerfile (Corrected)

```docker
# Stage 1: Builder
FROM python:3.10 AS builder

WORKDIR /app

COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY . .

# Stage 2: Final
FROM python:3.10-slim

WORKDIR /app

COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY --from=builder /app /app

CMD ["python", "app.py"]
```

---

## ▶️ Run Commands

```bash
docker build -f Dockerfile.multi -t flask-multi .
docker run -p 5000:5000 flask-multi
```

---

### Error:

ModuleNotFoundError: No module named 'flask'

### Reason:

Dependencies not installed in final stage

---

# 🔍 12. Useful Docker Commands

```bash
docker images
docker ps
docker logs <id>
docker exec -it <id> /bin/sh
```

---
