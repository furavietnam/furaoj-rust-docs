# Quickstart with Docker Compose

Deploy the complete FuraOJ v2.0 stack in less than 5 minutes on any Linux system running Docker Engine and Docker Compose v2.

## Prerequisites

- **Docker Engine:** v24.0+ (Tested on Docker 29+)
- **Docker Compose:** v2.20+ (Tested on Compose v5+)
- **Memory:** Minimum 2 GB RAM (4 GB recommended)
- **Disk:** 10 GB free space

## 1. Clone the Stack

```bash
git clone https://github.com/furavietnam/furaoj-rust.git
git clone https://github.com/furavietnam/furaoj-judgeserver-rust.git
cd furaoj-rust
```

## 2. Review Configuration

Ensure `config.json` and `judge_config.json` exist in the root directory:

```bash
cat config.json
cat judge_config.json
```

Notice: FuraOJ strictly uses **JSON-first configuration**. No `.env` files are required or supported.

## 3. Launch Services

Start all services with a single command:

```bash
docker compose up -d --build
```

Docker Compose will build and orchestrate:
- `furaoj_db`: PostgreSQL 16 database with persistent volume
- `furaoj_redis`: Redis 7 cache and pub/sub broker
- `furaoj_backend`: Rust Axum REST API and Tokio Bridge
- `furaoj_frontend`: React Vite SPA served via Nginx reverse proxy
- `furaoj_judge`: Sandboxed Rust Judge Server worker

## 4. Access the Application

- **Web Interface:** Open `http://localhost:3000`
- **REST API:** `http://localhost:8080/api/v2`
- **Default Administrator Credentials:** `admin` / `admin123`
