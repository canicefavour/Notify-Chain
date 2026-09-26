# Environment Setup Guide

## Quick Start

Before starting development on NotifyChain, run the environment checker to validate your setup:

```bash
npm run doctor
```

This will check all prerequisites and provide installation instructions for anything missing.

---

## Table of Contents

1. [Prerequisites Checker](#prerequisites-checker)
2. [Required Dependencies](#required-dependencies)
3. [Optional Dependencies](#optional-dependencies)
4. [Platform-Specific Setup](#platform-specific-setup)
5. [Troubleshooting](#troubleshooting)
6. [Verification](#verification)

---

## Prerequisites Checker

### Running the Check

```bash
# Using npm
npm run doctor

# Or directly
bash scripts/check-prereqs.sh

# Alternative alias
npm run check:env
```

### What Gets Checked

The prerequisites checker validates:

- ✅ **Core Tools**: Git, Make, Disk space
- ✅ **Node.js**: Version 16+ (18+ recommended)
- ✅ **npm**: Version 8+
- ✅ **Rust**: Version 1.70+ (for smart contracts)
- ✅ **Cargo**: Rust package manager
- ✅ **WebAssembly**: wasm32-unknown-unknown target
- ✅ **Stellar CLI**: (optional but recommended)
- ✅ **Project Dependencies**: node_modules installation
- ✅ **Environment Files**: .env configuration

### Exit Codes

| Code | Meaning |
|------|---------|
| `0` | All checks passed ✅ |
| `1` | One or more checks failed ❌ |

---

## Required Dependencies

### Node.js (Required)

NotifyChain requires Node.js for the listener service and dashboard.

**Minimum Version:** 16.0.0  
**Recommended Version:** 18.0.0 or later

#### Installation

**macOS (Homebrew):**
```bash
brew install node
```

**Linux (Ubuntu/Debian):**
```bash
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
sudo apt-get install -y nodejs
```

**Windows:**
Download from [nodejs.org](https://nodejs.org/)

**Using nvm (Recommended):**
```bash
# Install nvm
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.0/install.sh | bash

# Install Node.js 18
nvm install 18
nvm use 18
nvm alias default 18
```

#### Verification

```bash
node --version  # Should be v16+ (v18+ recommended)
npm --version   # Should be v8+
```

---

### Rust (Required for Smart Contracts)

Rust is required for building and testing Soroban smart contracts.

**Minimum Version:** 1.70.0

#### Installation

**All Platforms:**
```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

Follow the prompts, then restart your terminal or run:
```bash
source $HOME/.cargo/env
```

#### Install WebAssembly Target

Required for compiling Soroban contracts:
```bash
rustup target add wasm32-unknown-unknown
```

#### Verification

```bash
rustc --version  # Should be 1.70+
cargo --version
rustup target list --installed | grep wasm32
```

---

### Git (Required)

**Minimum Version:** Any recent version

#### Installation

**macOS:**
```bash
brew install git
```

**Linux (Ubuntu/Debian):**
```bash
sudo apt-get install git
```

**Windows:**
Download from [git-scm.com](https://git-scm.com/download/win)

#### Verification

```bash
git --version
```

---

## Optional Dependencies

### Stellar CLI (Recommended)

The Stellar CLI is used for deploying and interacting with Soroban contracts.

#### Installation

**Using Cargo:**
```bash
cargo install --locked stellar-cli --features opt
```

**macOS (Homebrew):**
```bash
brew install stellar/tap/stellar-cli
```

#### Verification

```bash
stellar --version
```

---

### Make (Optional but Useful)

Make is used for convenient build commands.

#### Installation

**macOS:**
```bash
xcode-select --install
```

**Linux (Ubuntu/Debian):**
```bash
sudo apt-get install build-essential
```

**Windows:**
```bash
choco install make
```

Or use WSL (Windows Subsystem for Linux).

#### Verification

```bash
make --version
```

---

## Platform-Specific Setup

### macOS

1. Install Homebrew (if not already installed):
   ```bash
   /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
   ```

2. Install dependencies:
   ```bash
   brew install node git
   ```

3. Install Rust:
   ```bash
   curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
   rustup target add wasm32-unknown-unknown
   ```

4. Install Stellar CLI:
   ```bash
   brew install stellar/tap/stellar-cli
   ```

5. Verify:
   ```bash
   npm run doctor
   ```

---

### Linux (Ubuntu/Debian)

1. Update package list:
   ```bash
   sudo apt-get update
   ```

2. Install Node.js:
   ```bash
   curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
   sudo apt-get install -y nodejs
   ```

3. Install Git and build tools:
   ```bash
   sudo apt-get install git build-essential
   ```

4. Install Rust:
   ```bash
   curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
   source $HOME/.cargo/env
   rustup target add wasm32-unknown-unknown
   ```

5. Install Stellar CLI:
   ```bash
   cargo install --locked stellar-cli --features opt
   ```

6. Verify:
   ```bash
   npm run doctor
   ```

---

### Windows

#### Option 1: Windows Subsystem for Linux (Recommended)

1. Install WSL2:
   ```powershell
   wsl --install
   ```

2. Follow the Linux setup instructions above inside WSL.

#### Option 2: Native Windows

1. Install Node.js from [nodejs.org](https://nodejs.org/)

2. Install Git from [git-scm.com](https://git-scm.com/download/win)

3. Install Rust:
   - Download from [rustup.rs](https://rustup.rs/)
   - Follow installer instructions

4. Install WebAssembly target:
   ```bash
   rustup target add wasm32-unknown-unknown
   ```

5. Install Stellar CLI:
   ```bash
   cargo install --locked stellar-cli --features opt
   ```

6. Verify:
   ```bash
   npm run doctor
   ```

---

## Project Setup

After all prerequisites are installed:

### 1. Clone Repository

```bash
git clone <repository-url>
cd Notify-Chain
```

### 2. Run Environment Check

```bash
npm run doctor
```

### 3. Install Project Dependencies

```bash
# Install listener dependencies
cd listener
npm install

# Install dashboard dependencies
cd ../dashboard
npm install

# Return to root
cd ..
```

### 4. Configure Environment Variables

```bash
# Create listener .env
cd listener
cp .env.example .env
# Edit .env with your configuration

# Create dashboard .env (if needed)
cd ../dashboard
cp .env.example .env
# Edit .env with your configuration

cd ..
```

### 5. Build Smart Contracts

```bash
cd contract
stellar contract build

# Or using cargo directly
cd contracts/hello-world
cargo build --target wasm32-unknown-unknown --release
```

### 6. Final Verification

```bash
npm run doctor
```

All checks should pass! ✅

---

## Troubleshooting

### Node.js Version Issues

**Problem:** Node.js version is too old

**Solution:**
```bash
# Using nvm (recommended)
nvm install 18
nvm use 18
nvm alias default 18

# Or upgrade via package manager
brew upgrade node  # macOS
sudo apt-get install --only-upgrade nodejs  # Linux
```

---

### Rust Installation Issues

**Problem:** Rust command not found after installation

**Solution:**
```bash
# Restart terminal or reload environment
source $HOME/.cargo/env

# Or add to shell profile permanently
echo 'source $HOME/.cargo/env' >> ~/.bashrc
echo 'source $HOME/.cargo/env' >> ~/.zshrc
```

---

### WebAssembly Target Missing

**Problem:** `wasm32-unknown-unknown` target not installed

**Solution:**
```bash
rustup target add wasm32-unknown-unknown
```

---

### Permission Issues (Linux/macOS)

**Problem:** Permission denied when installing global packages

**Solution:**
```bash
# Configure npm to use a different directory (recommended)
mkdir ~/.npm-global
npm config set prefix '~/.npm-global'
echo 'export PATH=~/.npm-global/bin:$PATH' >> ~/.bashrc
source ~/.bashrc

# Or use nvm to avoid permission issues
```

---

### Disk Space Issues

**Problem:** Not enough disk space

**Solution:**
```bash
# Clean npm cache
npm cache clean --force

# Clean cargo cache
cargo clean

# Remove unused Docker images/containers (if using Docker)
docker system prune -a
```

---

### Windows-Specific Issues

**Problem:** Scripts don't run on Windows

**Solution 1:** Use Git Bash (comes with Git for Windows)
```bash
# Run in Git Bash
bash scripts/check-prereqs.sh
```

**Solution 2:** Use Windows Subsystem for Linux (WSL)
```powershell
wsl --install
# Then use WSL terminal for all commands
```

---

## Verification

### Quick Health Check

Run this command to verify your entire setup:

```bash
npm run doctor
```

### Expected Output

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
  NotifyChain Environment Prerequisites Check
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

▶ System Information
────────────────────────────────────────────────────────────────────────────
ℹ Operating System: Darwin x86_64
ℹ CPU Cores: 8

▶ Core Development Tools
────────────────────────────────────────────────────────────────────────────
✓ Git 2.39.0
✓ Make 3.81
✓ Disk space: 50000MB available (>= 2000MB required)

▶ Node.js Ecosystem
────────────────────────────────────────────────────────────────────────────
✓ Node.js 18.16.0 (recommended >= 18.0.0)
✓ npm 9.5.0 (>= 8.0.0)

▶ Rust Ecosystem (Smart Contracts)
────────────────────────────────────────────────────────────────────────────
✓ Rust 1.75.0 (>= 1.70.0)
✓ Cargo 1.75.0
✓ WebAssembly target (wasm32-unknown-unknown) is installed
✓ Stellar CLI 21.0.0

▶ Project Dependencies
────────────────────────────────────────────────────────────────────────────
✓ Listener dependencies installed
✓ Dashboard dependencies installed
✓ listener/.env exists
✓ dashboard/.env exists

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
  Summary
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

  Total checks:    15
  Passed:          15
  Failed:          0
  Warnings:        0

✓ All checks passed! Your environment is ready.
```

---

## Continuous Integration

The environment checker can be integrated into CI/CD:

```yaml
# .github/workflows/test.yml
name: Tests

on: [push, pull_request]

jobs:
  environment-check:
    name: Environment Prerequisites
    runs-on: ubuntu-latest
    
    steps:
      - uses: actions/checkout@v3
      
      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '18'
      
      - name: Setup Rust
        uses: actions-rs/toolchain@v1
        with:
          toolchain: stable
          target: wasm32-unknown-unknown
      
      - name: Check Prerequisites
        run: npm run doctor
```

---

## Getting Help

If you encounter issues not covered here:

1. **Run the doctor script**: `npm run doctor`
2. **Check the output**: It provides specific installation commands
3. **Read the error messages**: They contain actionable instructions
4. **Check documentation**: README.md and other docs/
5. **Open an issue**: Include the output of `npm run doctor`

---

## Summary

### Quick Setup Checklist

- [ ] Install Node.js 18+
- [ ] Install Rust 1.70+
- [ ] Install Git
- [ ] Add wasm32-unknown-unknown target
- [ ] Run `npm run doctor`
- [ ] Install project dependencies
- [ ] Configure .env files
- [ ] Run `npm run doctor` again
- [ ] Start developing! 🚀

### Commands Reference

| Command | Purpose |
|---------|---------|
| `npm run doctor` | Check all prerequisites |
| `npm run check:env` | Alias for doctor |
| `npm install` | Install project dependencies |
| `npm run lint:config` | Check configuration drift |
| `npm run test:smoke` | Run smoke tests |

---

**Your environment is ready when `npm run doctor` shows all checks passed!** ✅
