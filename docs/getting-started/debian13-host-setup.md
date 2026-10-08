# Debian 13 Host Setup Guide

This guide walks through configuring a bare Debian 13 (Trixie) server for hosting FuraOJ v2.0 native development and container runtimes.

## 1. System Package Installation

Install essential compilation dependencies, PostgreSQL development headers, Seccomp libraries, and container tools:

```bash
sudo apt-get update && sudo apt-get install -y \
  build-essential \
  pkg-config \
  libseccomp-dev \
  libpq-dev \
  curl \
  git \
  ca-certificates \
  postgresql-client
```

## 2. Docker Engine Installation

Install official Docker packages for Debian:

```bash
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/debian/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/debian \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt-get update && sudo apt-get install -y \
  docker-ce \
  docker-ce-cli \
  containerd.io \
  docker-buildx-plugin \
  docker-compose-plugin
```

## 3. Rust and Node.js Toolchains (for Local Development)

If developing natively without Docker:

```bash
# Install Rust toolchain
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh -s -- -y
source "$HOME/.cargo/env"

# Install Node.js 22 LTS
curl -fsSL https://deb.nodesource.com/setup_22.x | sudo -E bash -
sudo apt-get install -y nodejs
```

## 4. Kernel Cgroups v2 & Seccomp Verification

Confirm cgroups v2 and Seccomp support in your Debian 13 kernel:

```bash
stat -fc %T /sys/fs/cgroup/
# Output should be: cgroup2fs

grep CONFIG_SECCOMP /boot/config-$(uname -r)
# Output should confirm: CONFIG_SECCOMP=y
```
