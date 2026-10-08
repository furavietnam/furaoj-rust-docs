# High-Level Architecture Overview

FuraOJ v2.0 represents a modern, modular online judge platform designed for sub-millisecond execution and horizontal scalability.

## Architectural Layers

1. **Presentation Layer (React Single Page Application):**
   - Built on React 18 and Vite.
   - 100% utility-first Tailwind CSS with zero SCSS.
   - Client-side routing with zero full-page reloads.
   - Integrated Monaco Editor and KaTeX LaTeX rendering.
   - Live WebSocket verdict updates via `/ws/live`.

2. **Application & API Layer (Rust Axum):**
   - Asynchronous Axum REST API v2 powered by Tokio.
   - Tower-HTTP middleware stack for tracing, CORS, and compression.
   - Cryptographic verification of PBKDF2/SHA256 password hashes.
   - Synchronous packet broadcast hub for connected WebSocket clients.

3. **Bridge Daemon (Tokio TCP Server):**
   - Listens on TCP port `9999`.
   - Big-endian 4-byte framing with zlib compression.
   - Manages live worker pools and submission queue routing.

4. **Judge Execution Layer (Rust Judge Server):**
   - Native Linux namespaces (`CLONE_NEWNET`, `CLONE_NEWPID`, `CLONE_NEWNS`).
   - cgroups v2 resource controllers.
   - Seccomp-BPF system call whitelisting.

5. **Storage & Persistence Layer:**
   - PostgreSQL 16 relational database with complete schema invariance matching 170+ original migrations.
   - Redis 7 cache and session broker.
