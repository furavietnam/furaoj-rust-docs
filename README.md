# FuraOJ v2.0 Documentation Suite

Welcome to the official technical documentation for **FuraOJ v2.0**, a next-generation competitive programming platform re-engineered from the ground up in Rust and modern React.

## System Topology & Highlights

- **High-Performance Rust Backend (`furaoj-rust`):** Asynchronous Axum REST API v2, Tokio multithreaded runtime, native PostgreSQL schema invariance, and PBKDF2/SHA256 password hash compatibility.
- **Modern React Single Page Application (`furaoj-rust/frontend`):** Built with React 18, Vite, and Tailwind CSS. Features Monaco Editor, KaTeX mathematical typesetting, zero full-page reloads, and real-time WebSocket verdict streaming.
- **Memory-Safe Rust Judge Server (`furaoj-judgeserver-rust`):** Linux Seccomp-BPF syscall whitelisting, cgroups v2 resource accounting, network/PID/IPC namespaces, and zlib wire protocol.
- **Docker-First Architecture:** Complete orchestration via Docker Compose with JSON configuration volumes (`config.json`, `judge_config.json`) and zero `.env` dependencies.

## Browsing Documentation

Open `docs/index.html` in any web browser, or serve locally using any static HTTP server:

```bash
npx serve docs
# or
python3 -m http.server --directory docs 8000
```
