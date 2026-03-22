# 🔐 JWTShield — JWT Analyzer & Tester

> **Project 11** · Python Security Tooling Series  
> *Decode · Audit · Brute-Force · Forge · Attack Simulation*

---

## 📌 Project Name

**JWTShield**

## 📝 Short Description

A browser-based JWT (JSON Web Token) security analyzer and pen-testing toolkit. Paste any JWT to instantly decode it, audit it against 20+ security checks, brute-force weak HMAC secrets using a built-in wordlist, forge tampered tokens, simulate real-world attack vectors, and generate production-ready Python/PyJWT fix code — all with zero backend, running entirely in the browser via the Web Crypto API.

---

## 🚀 Features

### 🔍 Analyze Tab
- **Live token decoder** — color-coded header (red) · payload (orange) · signature (purple) display
- **Human timestamp visualizer** — converts raw Unix `iat`, `exp`, `nbf` to UTC dates with live countdown ("Expires in 4m 32s")
- **Security score ring** — 0–100 score with Secure / Needs Work / Vulnerable verdict
- **20+ security checks** across 4 severity levels:

| Severity | Checks |
|----------|--------|
| 🔴 Critical | `alg:none` bypass, missing `exp`, sensitive data in payload, suspicious `kid` injection, empty signature |
| 🟡 Warning | Weak HMAC (HS256), expired token, excessive TTL (>30d), algorithm confusion risk, privilege escalation claim, oversized payload |
| 🔵 Info | Missing `iss`, `aud`, `nbf`, `jti`, `kid`, `typ` |
| 🟢 OK | Strong asymmetric algorithm, complete claim set |

- **Verify Signature** — enter any known secret and instantly verify if it matches (HS256/384/512)
- **Python/PyJWT fix generator** — context-aware secure code tailored to the token's issues
- **Export JSON report** — full machine-readable report with all findings

### 🔨 Wordlist Brute-Force
- Runs entirely in-browser using **Web Crypto API** (`HMAC-SHA256/384/512`)
- Three modes: **Top 100**, **Top 500**, **Custom wordlist**
- Real-time progress bar, speed (words/sec), elapsed time, estimated remaining time
- On crack: shows secret, auto-generates Python secret rotation code with token invalidation strategy
- Supports `HS256`, `HS384`, and `HS512` — automatically detected from token header

### ⚗️ Forge / Tamper Tab
- Full **claim editor** for both header and payload (add/remove/edit any claim)
- Live HMAC signing with custom secret (HS256/384/512) or `alg:none`
- **Send forged token directly to Analyzer** in one click
- **Quick Presets**: Admin Escalation, 30-day Expiry, No Expiry, alg:none, Minimal Claims, RS256 Header
- Load current analyzed token into forge editor

### 💀 Attack Simulation Tab
Six real-world attack demos — generates live attack tokens with full explanation:

| Attack | Description |
|--------|-------------|
| `alg:none` Bypass | Removes signature, sets alg to none, forges admin claims |
| Algorithm Confusion | RS256 → HS256 using public key as HMAC secret |
| `kid` Injection | SQL injection + path traversal in the key ID header |
| Weak Secret Brute-Force | Signs with `"secret"` — cracks instantly in Analyzer |
| Replay Attack | No `exp`, no `jti` — token valid forever |
| Privilege Escalation | Tampered `role: "admin"` with full permissions array |

Each demo token can be sent directly to the Analyzer tab.

### 📜 History Tab
- Stores last 50 analyzed tokens in `localStorage`
- Shows algorithm badge, subject, security score, critical count, timestamp
- Click any entry to reload it in the Analyzer

---

## 🛠️ Tech Stack

| Layer | Technology |
|-------|-----------|
| Frontend | Vanilla HTML/CSS/JS (zero dependencies) |
| Cryptography | **Web Crypto API** — `crypto.subtle.importKey` + `HMAC-SHA256/384/512` |
| Python target | **PyJWT** + **cryptography** library |
| Fonts | JetBrains Mono (code) · Syne (UI) |

---

## 🐍 Python Stack (Target Fix Code)

The tool generates fix code for:

```bash
pip install PyJWT cryptography
```

**Key patterns generated:**
- `jwt.encode()` with RS256 + `kid` header
- `jwt.decode()` with explicit algorithm allowlist, required claims, audience + issuer validation
- `secrets.token_hex(32)` — 256-bit random secret generation
- Redis-backed token revocation (`setex` / `exists`)
- `kid` sanitization with `re.match(r'^[a-zA-Z0-9\-_]{1,64}$', kid)`
- Token version invalidation pattern for mass rotation

---

## 📦 Sample Tokens Included

| Token | Purpose |
|-------|---------|
| `alg:none` Attack | Demonstrates signature bypass |
| Weak HS256 (`"secret"`) | Demonstrates brute-force — cracks in <1s |
| Sensitive Data | Password + credit card in payload |
| `kid` Injection | SQL injection in key ID header |
| Expired + Minimal | No iss/aud/jti, already expired |
| Well-formed RS256 | All best practices — shows green score |

---

## 🔒 Security Concepts Covered

- JWT structure (header · payload · signature)
- Base64url encoding (NOT encryption)
- HMAC-SHA256/384/512 symmetric signing
- RSA-2048 / ES256 asymmetric signing
- `alg:none` CVE pattern
- Algorithm confusion attack (RS256 → HS256)
- `kid` SQL injection and path traversal
- Offline brute-force against HMAC tokens
- Token replay without `jti` denylist
- Privilege escalation via payload tampering
- JWE (JSON Web Encryption) — for sensitive payloads
- Token revocation strategies
- Key rotation with `kid`

---

## 🚦 Usage

1. Open `jwt_analyzer.html` in any modern browser (Chrome, Firefox, Edge, Safari)
2. No server required — fully client-side
3. Paste a JWT or click a sample token
4. Click **⚡ Analyze** to run all checks
5. For HMAC tokens, scroll down to run **Wordlist Brute-Force**
6. Switch to **⚗️ Forge** to create tampered tokens
7. Switch to **💀 Attack Sim** to generate and study attack tokens

---

## 📁 File Structure

```
project-11-jwtshield/
│
├── index.html     # Main application (single file, no dependencies) - jwt_analyzer
├── README.md             # This file
└── jwt_analyzer.docx     # Project documentation
```

---

## ⚠️ Disclaimer

This tool is intended for **educational purposes** and **authorized security testing only**. Use it to audit your own tokens and applications, or in environments where you have explicit permission. Do not use against systems you do not own.

---

## 👤 Author

**Project 11** of the Python Security Tooling Series  
Built with Web Crypto API · Targets Python/PyJWT ecosystem

---

## 📄 License

MIT License — free for personal and commercial use.
