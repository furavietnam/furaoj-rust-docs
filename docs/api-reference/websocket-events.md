# Real-Time WebSocket Event Stream

The FuraOJ real-time WebSocket endpoint is available at `/ws/live`.

## Connection Flow

Clients establish a standard WebSocket upgrade handshake:

```
GET /ws/live HTTP/1.1
Host: localhost:8080
Upgrade: websocket
Connection: Upgrade
```

## Streamed Event Types

### `submission_update`
Emitted when an overall submission status changes:
```json
{
  "type": "submission_update",
  "submission_id": 1042,
  "verdict": "AC",
  "time_ms": 14,
  "memory_kb": 1980,
  "score": 100
}
```

### `case_update`
Emitted as each individual test case finishes grading:
```json
{
  "type": "case_update",
  "submission_id": 1042,
  "case_index": 1,
  "case_verdict": "AC",
  "payload": {
    "time_ms": 2,
    "memory_kb": 1420,
    "points": 20
  }
}
```

### `contest_update`
Emitted when contest rankings or scoreboard states update:
```json
{
  "type": "contest_update",
  "payload": {
    "contest_id": 1,
    "slug": "demo",
    "updated_at": "2026-10-09T00:00:00Z"
  }
}
```
