<!-- Unified Documentation Sidebar -->

* [FuraOJ v2.0 Overview](README.md)

* **Getting Started**
  * [Quickstart with Docker Compose](getting-started/quickstart-docker.md)
  * [Debian 13 Host Setup](getting-started/debian13-host-setup.md)
  * [JSON Configuration Reference](getting-started/json-configuration.md)

* **System Architecture**
  * [Architecture Overview](architecture/overview.md)
  * [Rust Backend (Axum)](architecture/backend-axum.md)
  * [Frontend SPA (React & Vite)](architecture/frontend-react-vite.md)
  * [Bridge Daemon & WebSockets](architecture/bridge-daemon.md)

* **Database & Authentication**
  * [PostgreSQL Schema Invariance](database/schema-invariance.md)
  * [Django PBKDF2 Password Verification](database/django-pbkdf2-auth.md)
  * [Database Tables Reference](database/tables-reference.md)

* **Judge Engine & Sandboxing**
  * [Configuring the Rust Judge](judge-system/setting-up-judge.md)
  * [Linux Sandboxing & Security](judge-system/linux-sandboxing.md)
  * [TCP Wire Protocol & zlib](judge-system/wire-protocol.md)
  * [Supported Compilers & Runtimes](judge-system/supported-languages.md)

* **Problem & Contest Formats**
  * [Problem Directory Specification](problem-format/specification.md)
  * [Custom Output Checkers](problem-format/custom-checkers.md)
  * [Interactive Grader Pipelines](problem-format/interactive-graders.md)
  * [ICPC Contest Format](contest-formats/acm-icpc.md)
  * [IOI Subtask Scoring](contest-formats/ioi-subtasks.md)

* **API & Integrations**
  * [Axum REST API v2](api-reference/rest-v2.md)
  * [WebSocket Event Streams (/ws/live)](api-reference/websocket-events.md)
