# Linux Sandboxing & Kernel Security

FuraOJ v2.0 enforces multi-layered process isolation using native Linux kernel primitives, replacing legacy user-space execution sandboxes.

## 1. Linux Namespaces

Untrusted student code runs inside an unshared namespace environment:
- **`CLONE_NEWNET`:** Completely disables network sockets and networking stacks. Prevents data exfiltration and reverse shells.
- **`CLONE_NEWPID`:** Isolates process tables. Untrusted code cannot observe, trace, or signal other processes running on the host.
- **`CLONE_NEWIPC` & `CLONE_NEWUTS`:** Blocks inter-process shared memory and hostname changes.

## 2. cgroups v2 (Control Groups)

- **`memory.max`:** Hard ceiling enforced by the Linux kernel memory controller. Out-of-memory processes are terminated with a `MLE` verdict.
- **`pids.max`:** Restricts maximum process count to prevent fork-bomb denial-of-service exploits.
- **`cpu.max`:** Enforces fair-share CPU quota.

## 3. Seccomp-BPF Syscall Filtering

The `libseccomp` engine restricts available system calls to an immutable whitelist of safe instructions:
- **Allowed:** `read`, `write`, `exit_group`, `brk`, `mmap`, `clock_gettime`, `getrusage`, `fstat`.
- **Blocked & Killed:** `socket`, `connect`, `bind`, `ptrace`, `kill`, `reboot`, `mount`. Any illegal system call triggers immediate process termination.
