# TierFuraOJ Universal Language Support Specification

## 1. Overview & Objectives
The FuraOJ Rust Judge Server (`furaoj-judgeserver-rust`) provides native, zero-overhead executor and language compatibility for TierFuraOJ. Over 60 distinct programming language configurations and execution environments are supported, including specialized Vietnamese competitive programming contest configurations (`CPPTHEMIS`, `PASTHEMIS`), visual Scratch 3.0 programming (`SCRATCH`), and optimized precompiled header suites.

- **Organization:** `furavietnam` (`https://github.com/furavietnam`)
- **Primary Contributor:** `nmkdeveloper` (`Nguyen Minh Khoi <nguyenminhkhoi.nmk.dev@gmail.com>`)
- **Host Target:** Debian 13 (Trixie) and containerized judge workers.

---

## 2. Vietnamese Contest Themis Compatibility (`CPPTHEMIS` & `PASTHEMIS`)

Themis is the standard grading environment used throughout Vietnamese competitive programming, including national Olympiads (VOI), provincial competitions (HSG tỉnh), and Tin học trẻ contests. Many contestant solutions rely on Themis preprocessor macros and expanded call stacks for deep recursion.

### 2.1 C++ Themis (`CPPTHEMIS`)
- **Language Key:** `CPPTHEMIS`
- **Standard:** C++14
- **Preprocessor Defines:** `-DTHEMIS`
- **Stack Allocation:** Expanded 64 MB (66,060,288 bytes) stack size via GNU Linker flags (`-Wl,-z,stack-size=66060288`) to prevent segmentation faults during deep recursion / tree traversal.
- **Compilation Command:**
  ```bash
  g++ -std=c++14 -pipe -O2 -s -static -lm -x c++ -Wl,-z,stack-size=66060288 -DTHEMIS -o {output_binary} {source_file}
  ```
- **Contest Pattern:**
  Contestants frequently write:
  ```cpp
  #ifdef THEMIS
  freopen("task.inp", "r", stdin);
  freopen("task.out", "w", stdout);
  #endif
  ```
  The judge worker mirrors standard file I/O transparently.

### 2.2 Free Pascal Themis (`PASTHEMIS`)
- **Language Key:** `PASTHEMIS`
- **Compiler:** Free Pascal (`fpc`)
- **Preprocessor Defines:** `-dTHEMIS`
- **Stack Allocation:** `-Cs66060288` (64 MB stack)
- **Compilation Command:**
  ```bash
  fpc -Fe/dev/stderr -dTHEMIS -O2 -XS -Sg -Cs66060288 -o{output_binary} {source_file}
  ```

### 2.3 Scratch 3.0 Visual Programming (`SCRATCH`)
- **Language Key:** `SCRATCH`
- **Source Extension:** `sb3` (Scratch 3.0 project archive)
- **Binary Runner:** `scratch-run`
- **Pre-Execution Validation:**
  Every submission is validated before test evaluation via:
  ```bash
  scratch-run --check {submission_file.sb3}
  ```
  If validation exits non-zero with `Not a valid Scratch file`, the submission immediately receives `CompileError`.
- **Runtime Sandboxing & Syscall Whitelist:**
  Requires the following kernel capabilities and syscalls:
  - Whitelist: `capget`, `eventfd2`, `shutdown`, `pkey_alloc`, `pkey_free`, `io_uring_setup`.
  - Address space grace: 1 MB (1,048,576 bytes).
  - Validation limits: 10 seconds timeout, 256 MB memory limit.

### 2.4 Legacy Pascal Compatibility Unit (`Windows.pas`)
Legacy Themis test data often contain Free Pascal / Delphi custom checkers that depend on the Windows API unit (`uses Windows;`) to manage working directories:
- **Compatibility Unit:** Precompiled `Windows.pas` stub installed into `/usr/lib/fpc/Windows.pas`:
  ```pascal
  unit Windows;
  interface
  type
    DWORD = LongWord;
    LPWSTR = PWideChar;
    LPCWSTR = PWideChar;
    BOOL = LongBool;
  function GetCurrentDirectoryW(nBufferLength: DWORD; lpBuffer: LPWSTR): DWORD;
  function SetCurrentDirectoryW(lpPathName: LPCWSTR): BOOL;
  implementation
  uses SysUtils;
  function GetCurrentDirectoryW(nBufferLength: DWORD; lpBuffer: LPWSTR): DWORD;
  var s: UnicodeString;
  begin
    GetDir(0, s);
    StrPLCopy(lpBuffer, s, nBufferLength);
    Exit(Length(lpBuffer));
  end;
  function SetCurrentDirectoryW(lpPathName: LPCWSTR): BOOL;
  begin
    ChDir(lpPathName);
    Exit(True);
  end;
  end.
  ```

### 2.5 Precompiled Headers (PCH) Acceleration
To minimize compilation overhead across competitive programming workloads, the TierFuraOJ environment precompiles essential header files:
1. **Precompiled `bits/stdc++.h.gch`:**
   Precompiled for 8 C++ standard variants with `-Wall -DONLINE_JUDGE -O2 -fmax-errors=5 -march=native -s`:
   - `cpp03`: `g++ -std=c++03`
   - `cpp11`: `g++ -std=c++11`
   - `cpp14`: `g++ -std=c++14`
   - `cpp17`: `g++ -std=c++17`
   - `cpp20`: `g++ -std=c++20`
   - `cpp23`: `g++ -std=c++23`
   - `cppicpc`: `g++ -std=gnu++20 -O2 -s -static`
   - `cppthemis`: `g++ -std=c++14 -pipe -O2 -s -static -DTHEMIS`
2. **Precompiled Testlib Suite:**
   - Standard testlib: `/usr/include/testlib.h` precompiled with C++17.
   - Dual-Mode Vietnamese Contest testlib: `/usr/include/testlib_themis_cms.h` precompiled into three GCH targets:
     * `/usr/include/testlib_themis_cms.h.gch/themis` (flag `-DTHEMIS`)
     * `/usr/include/testlib_themis_cms.h.gch/cms` (flag `-DCMS`)
     * `/usr/include/testlib_themis_cms.h.gch/testlib` (standard mode)

---

## 3. Comprehensive TierFuraOJ Language Catalog

| Key | Display Name | Family | Extension | Compiler / Command | Key Compilation / Execution Flags | Time Multiplier |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| `C` | C (GCC) | C/C++ | `c` | `gcc` | `-O2 -std=c11 -lm` | 1.0x |
| `C11` | C11 (GCC) | C/C++ | `c` | `gcc` | `-O2 -std=gnu11 -static -lm` | 1.0x |
| `C23` | C23 (GCC) | C/C++ | `c` | `gcc` | `-O2 -std=gnu23 -static -lm` | 1.0x |
| `CICPC` | C (ICPC) | C/C++ | `c` | `gcc` | `-O2 -std=gnu11 -static -lm` | 1.0x |
| `CPP03` | C++03 (GCC) | C/C++ | `cpp` | `g++` | `-O2 -std=c++03 -static -lm` | 1.0x |
| `CPP11` | C++11 (GCC) | C/C++ | `cpp` | `g++` | `-O2 -std=c++11 -static -lm` | 1.0x |
| `CPP14` | C++14 (GCC) | C/C++ | `cpp` | `g++` | `-O2 -std=c++14 -static -lm` | 1.0x |
| `CPP17` | C++17 (GCC) | C/C++ | `cpp` | `g++` | `-O2 -std=c++17 -static -lm` | 1.0x |
| `CPP20` | C++20 (GCC) | C/C++ | `cpp` | `g++` | `-O2 -std=c++20 -static -lm` | 1.0x |
| `CPP23` | C++23 (GCC) | C/C++ | `cpp` | `g++` | `-O2 -std=c++23 -static -lm` | 1.0x |
| `CPPICPC` | C++ (ICPC) | C/C++ | `cpp` | `g++` | `-O2 -std=gnu++20 -static -lm` | 1.0x |
| `CPPTHEMIS` | C++ (Themis) | C/C++ | `cpp` | `g++` | `-std=c++14 -pipe -O2 -s -static -lm -Wl,-z,stack-size=66060288 -DTHEMIS` | 1.0x |
| `CLANG` | C (Clang) | C/C++ | `c` | `clang` | `-O2 -std=c11 -lm` | 1.0x |
| `CLANGX` | C++ (Clang++) | C/C++ | `cpp` | `clang++` | `-O2 -std=c++17 -lm` | 1.0x |
| `CLPP14` | C++14 (Clang++) | C/C++ | `cpp` | `clang++` | `-O2 -std=c++14 -lm` | 1.0x |
| `CLPP17` | C++17 (Clang++) | C/C++ | `cpp` | `clang++` | `-O2 -std=c++17 -lm` | 1.0x |
| `CLPP20` | C++20 (Clang++) | C/C++ | `cpp` | `clang++` | `-O2 -std=c++20 -lm` | 1.0x |
| `CLPP23` | C++23 (Clang++) | C/C++ | `cpp` | `clang++` | `-O2 -std=c++23 -lm` | 1.0x |
| `PAS` | Pascal (FPC) | Pascal | `pas` | `fpc` | `-O2 -Fe/dev/stderr` | 1.0x |
| `PASTHEMIS` | Pascal (Themis) | Pascal | `pas` | `fpc` | `-O2 -dTHEMIS -XS -Sg -Cs66060288 -Fe/dev/stderr` | 1.0x |
| `SCRATCH` | Scratch 3.0 | Visual | `sb3` | `scratch-run` | `--check` syntax validation, 1MB address grace | 1.0x |
| `PY2` | Python-Legacy | Script | `py` | `py2` | `-B` (time multiplier: 3.0x) | 3.0x |
| `PY3` | Python 3 | Python | `py` | `python3` | `-B` (time multiplier: 2.5x) | 2.5x |
| `PYPY` | PyPy 2 | Python | `py` | `pypy` | `-B` (JIT warmup grace) | 1.5x |
| `PYPY3` | PyPy 3 | Python | `py` | `pypy3` | `-B` (JIT warmup grace) | 1.5x |
| `JAVA8` | Java 8 | JVM | `java` | `javac` / `java` | `-Xmx256M -Xss64M` | 2.0x |
| `JAVA` | Java 17/21/25 | JVM | `java` | `javac` / `java` | `-Xmx256M -Xss64M` | 2.0x |
| `KOTLIN` | Kotlin | JVM | `kt` | `kotlinc` / `java` | `-script` runtime execution | 2.0x |
| `SCALA` | Scala | JVM | `scala` | `scalac` / `scala` | JVM execution wrapper | 2.0x |
| `GROOVY` | Groovy | JVM | `groovy` | `groovyc` / `groovy` | JVM execution wrapper | 2.0x |
| `RUST` | Rust | Native | `rs` | `rustc` | `-O --edition 2021` | 1.0x |
| `GO` | Go | Native | `go` | `go build` | `-ldflags="-s -w"` | 1.0x |
| `D` | D | Native | `d` | `gdc` | `-O2` | 1.0x |
| `ZIG` | Zig | Native | `zig` | `zig build-exe` | `-O ReleaseFast` | 1.0x |
| `SWIFT` | Swift | Native | `swift` | `swiftc` | `-O` | 1.0x |
| `NODEJS` | Node.js | Script | `js` | `node` | `--max-old-space-size=256` | 2.0x |
| `V8JS` | JavaScript (V8) | Script | `js` | `d8` | Native V8 shell | 1.5x |
| `BASH` | Bash Shell | Script | `sh` | `bash` | Sandbox shell runner | 1.0x |
| `AWK` | AWK | Script | `awk` | `gawk` | `-f` | 1.0x |
| `SED` | Sed | Script | `sed` | `sed` | `-f` | 1.0x |
| `PERL` | Perl | Script | `pl` | `perl` | Script execution | 2.0x |
| `PHP` | PHP | Script | `php` | `php` | CLI interpreter | 2.0x |
| `RUBY` | Ruby | Script | `rb` | `ruby` | Script execution | 2.0x |
| `LUA` | Lua | Script | `lua` | `lua` | Lua 5.4 interpreter | 1.0x |
| `TCL` | TCL | Script | `tcl` | `tclsh` | TCL interpreter | 1.0x |
| `HASK` | Haskell | Functional | `hs` | `ghc` | `-O2` | 1.5x |
| `OCAML` | OCaml | Functional | `ml` | `ocamlopt` | `-O2` | 1.2x |
| `SCM` | Scheme | Functional | `scm` | `guile` | Scheme interpreter | 1.5x |
| `RKT` | Racket | Functional | `rkt` | `racket` | Racket bytecode runner | 1.5x |
| `SBCL` | Common Lisp | Functional | `lisp` | `sbcl` | `--script` | 1.2x |
| `PRO` | Prolog | Logic | `pro` | `swipl` | `-q -t halt -s` | 2.0x |
| `TEXT` | Plain Text | Text | `txt` | `cat` | Raw test/diff compare | 1.0x |

---

## 4. Rust Judge Server Implementation Architecture

In `furaoj-judgeserver-rust`, language compilation and sandboxing are unified through the `LanguageExecutor` trait:

```rust
// Logic: Universal trait defining compilation, sandboxing, and runtime behaviors.
// Input: Source file path, compile flags, target limits.
// Output: Command line vectors and environment constraints for execution.
pub trait LanguageExecutor: Send + Sync {
    fn key(&self) -> &'static str;
    fn display_name(&self) -> &'static str;
    fn source_extension(&self) -> &'static str;
    fn compile_command(&self, source_path: &Path, output_path: &Path) -> Option<Vec<String>>;
    fn run_command(&self, binary_path: &Path) -> Vec<String>;
    fn time_multiplier(&self) -> f64 { 1.0 }
    fn memory_grace_mb(&self) -> u64 { 0 }
    fn extra_syscall_whitelist(&self) -> &'static [&'static str] { &[] }
    fn pre_validation(&self, _source_path: &Path) -> Result<(), String> { Ok(()) }
}
```

The `LanguageRegistry` singleton indexes all executors and provides case-insensitive lookup aliases (`cpp` -> `CPP20`, `python3` -> `PY3`, `themis` -> `CPPTHEMIS`, `pas_themis` -> `PASTHEMIS`, `sb3` -> `SCRATCH`). During the Bridge handshake sequence, all supported executor keys are announced dynamically.
