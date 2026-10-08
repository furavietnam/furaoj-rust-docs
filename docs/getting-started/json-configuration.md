# JSON Configuration Architecture (Zero .env)

FuraOJ v2.0 enforces a strict **Zero .env Policy**. All configuration parameters are strictly declared in typed JSON format.

## Core Directives

1. **Deterministic Structure:** JSON schemas are strictly validated on application startup.
2. **Deep Merge Overrides:** Developers create a `local_settings.json` file to override keys without modifying source control.
3. **No Hidden State:** No magic environment variable parsing or dotenv libraries.

## 1. Backend Configuration (`config.json`)

```json
{
  "database": {
    "url": "postgres://furaoj:furaoj_secure_password@127.0.0.1:5432/furaoj",
    "max_connections": 20,
    "min_connections": 5
  },
  "redis": {
    "url": "redis://127.0.0.1:6379"
  },
  "server": {
    "host": "0.0.0.0",
    "port": 8080
  },
  "bridge": {
    "host": "0.0.0.0",
    "port": 9999,
    "auth_key": "judge_secret_authentication_key"
  },
  "auth": {
    "jwt_secret": "furaoj_ultra_secure_jwt_secret_token_1234567890",
    "jwt_expiration_hours": 72
  }
}
```

## 2. Local Settings Overrides (`local_settings.json`)

To change database credentials on a development machine:

```json
{
  "database": {
    "url": "postgres://custom_user:custom_pass@localhost:5432/my_dev_db"
  }
}
```

The Rust backend performs a recursive JSON merge upon booting.

## 3. Judge Configuration (`judge_config.json`)

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
