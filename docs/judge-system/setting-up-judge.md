# Configuring the Rust Judge Server

The `furaoj-judgeserver-rust` daemon connects to the FuraOJ Bridge and securely executes submitted solutions inside an isolated Linux sandbox.

## 1. Configuration File (`judge_config.json`)

The judge worker reads its configuration from `judge_config.json`:

```json
{
  "judge": {
    "name": "default-judge",
    "key": "judge_secret_authentication_key",
    "bridge_host": "127.0.0.1",
    "bridge_port": 9999,
    "problem_dir": "./problems",
    "sandbox_tmpfs_size_mb": 64
  },
  "limits": {
    "max_cpu_time": 10.0,
    "max_memory_mb": 1024,
    "max_output_mb": 16
  }
}
```

## 2. Launching the Judge Worker

Run natively with:

```bash
cargo run --release -- judge_config.json
```

Or run via Docker:

```bash
docker run -d \
  --name furaoj_judge \
  --privileged \
  -v ./judge_config.json:/app/judge_config.json:ro \
  -v /sys/fs/cgroup:/sys/fs/cgroup:rw \
  furavietnam/furaoj-judgeserver-rust:latest
```

## 3. Handshake & Registration

Upon launching, the judge worker connects to `bridge_host:bridge_port`, performs a cryptographic handshake verifying the shared `auth_key`, reports available compilers (`gcc`, `g++`, `rustc`, `python3`), and enters the evaluation polling loop.
