# FuraOJ v2.0 - Technical Documentation

**FuraOJ v2.0** is an enterprise-grade competitive programming and algorithmic evaluation platform completely rewritten in **Rust** and modern **React**.

## Architectural Pillars

```
+---------------------------------------------------------------------------------+
|                                 CLIENT BROWSERS                                 |
|            React 18 SPA (Vite + Tailwind CSS + Monaco Editor + KaTeX)            |
+----------------------------------------+----------------------------------------+
                                         |
                                         | HTTP / REST API v2 & WebSocket (/ws/live)
                                         v
+---------------------------------------------------------------------------------+
|                          FURAOJ RUST BACKEND (Axum)                             |
|  - Tokio Multithreaded Async Runtime                                            |
|  - Tower-HTTP Middleware & CORS                                                 |
|  - PBKDF2/SHA256 Django Password Authentication Engine                          |
|  - Real-Time WebSocket Hub                                                      |
|  - Tokio TCP Bridge Server (Port 9999)                                          |
+-------------------+-------------------------------------+-----------------------+
                    |                                     |
                    | SQL (sqlx pool)                     | zlib Wire Protocol (TCP :9999)
                    v                                     v
+-------------------------------+     +-------------------------------------------+
|    POSTGRESQL 16 CLUSTER      |     |       RUST JUDGE SERVER WORKER            |
| - 1:1 Schema Invariance       |     | - Linux cgroups v2 Limits                 |
| - 170+ Django Migrations      |     | - Seccomp-BPF Syscall Filter              |
| - Persistent Tables & Indices |     | - Network, PID, & IPC Namespaces          |
+-------------------------------+     | - GCC 13, Clang, Rustc, Python 3.12       |
                                      +-------------------------------------------+
```

## Key Architectural Advantages

1. **Native PostgreSQL Schema Invariance:** Eliminates any database schema changes or user data migrations. Directly operates on existing tables (`auth_user`, `judge_problem`, `judge_submission`, etc.) without requiring password resets.
2. **Sub-Millisecond Evaluation Latency:** Direct memory-safe Tokio TCP socket communication between backend and judge worker eliminates message broker overhead.
3. **Zero Full-Page Reloads:** High-contrast dark theme Single Page Application with client-side routing, Monaco IDE, and KaTeX LaTeX mathematics.
4. **Linux Kernel Sandboxing:** Robust isolation using native Linux namespaces (`CLONE_NEWNET`, `CLONE_NEWPID`), cgroups v2 memory and CPU controllers, and Seccomp-BPF filters.
5. **Strict JSON Configuration:** Zero `.env` files. Transparent, verifiable JSON configuration files (`config.json`, `local_settings.json`, `judge_config.json`).
