# TCP Wire Protocol & zlib Packet Framing

Communication between the Rust backend Bridge (TCP :9999) and the Rust Judge Server worker utilizes a binary framed wire protocol.

## Packet Framing

Every packet transmitted across the TCP stream begins with a 4-byte big-endian unsigned length header (`u32::to_be_bytes()`):

```
+-----------------------------------+-----------------------------------+
|  4-Byte Big-Endian Length (u32)   |    zlib-Compressed JSON Payload   |
+-----------------------------------+-----------------------------------+
```

## Compression

The packet payload is a serialized UTF-8 JSON document compressed via `zlib` at standard compression levels using `flate2::write::ZlibEncoder`.

## Packet Lifecycle

1. **Handshake:**
   ```json
   {
     "name": "default-judge",
     "key": "judge_secret_authentication_key",
     "problems": {},
     "executors": ["gcc", "g++", "rustc", "python3"],
     "version": "2.0.0"
   }
   ```
2. **Ping / Pong:**
   - Bridge sends `{"name": "ping"}`
   - Judge responds `{"name": "ping-response", "when": 1728000000}`
3. **Submission Evaluation:**
   - Bridge emits `submission-request`
   - Judge acknowledges `submission-acknowledged`
   - Judge begins evaluation `grading-begin`
   - Judge streams testcase results `test-case-status`
   - Judge completes evaluation `grading-end`
