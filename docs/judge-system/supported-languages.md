# Supported Compilers & Execution Runtimes

FuraOJ v2.0 provides out-of-the-box support for modern competitive programming languages and toolchains.

## Language Matrix

| Language | Identifier | Toolchain | Standard Flags |
| :--- | :--- | :--- | :--- |
| **C++ 20** | `cpp`, `c++` | GCC 13 (g++) | `-O3 -std=c++20 -Wall -Wextra` |
| **C 11** | `c` | GCC 13 (gcc) | `-O3 -std=c11 -Wall` |
| **Rust 2021** | `rust`, `rs` | rustc | `-O --edition 2021` |
| **Python 3** | `python`, `py`, `python3` | CPython 3.12 | `python3 -m py_compile` |
| **Java 21** | `java` | OpenJDK 21 | `javac -encoding UTF-8` |

## Resource Accounting & Limits

- **Time Limit Enforcement:** Monitored via high-precision monotonic clock and POSIX `RLIMIT_CPU`. Overages result in `TLE`.
- **Memory Limit Enforcement:** Monitored via Linux cgroups v2 `memory.max` and POSIX `RLIMIT_AS`. Overages result in `MLE`.
- **Output Limit Enforcement:** Monitored via `RLIMIT_FSIZE` (default 16 MB). Overages result in `OLE`.
