# Bridge Daemon & WebSocket Broadcaster

The Bridge Daemon acts as the high-throughput synchronization hub connecting backend controllers, evaluation judge servers, and live browser sessions.

## Bridge Server Architecture

- **Listener:** Tokio asynchronous TCP socket listener on port `9999`.
- **Worker Management:** Maintains connected judge worker pool with round-robin dispatch.
- **Wire Framing:**
  - 4-byte big-endian unsigned length header (`u32`).
  - zlib-compressed JSON payload using the `flate2` crate.
- **Packet Streaming:**
  - When a judge worker emits evaluation updates (`grading-begin`, `test-case-status`, `grading-end`), the Bridge streams updates directly into the Axum WebSocket Hub.
  - Connected browser clients subscribed to `/ws/live` receive instantaneous JSON push updates.
