# 🔐 OAuth 2.0 Misconfiguration — Missing PKCE Enforcement

![Security](https://img.shields.io/badge/security-OAuth%202.0-red)
![Severity](https://img.shields.io/badge/severity-Medium-orange)
![Status](https://img.shields.io/badge/status-Fixed-success)

## 📌 Overview

During a responsible security assessment, I identified an **OAuth 2.0 security misconfiguration** affecting a web application.

The vulnerability was reported through the vendor's official **bug bounty program** and has since been **remediated**.

The issue was caused by a combination of:

- Improper OAuth client registration controls
- Missing PKCE enforcement
- Insufficient authorization flow protections

---

# 🧩 Vulnerability Details

| Field | Information |
|------|-------------|
| **Type** | OAuth 2.0 Security Misconfiguration |
| **Category** | Authentication / Business Logic |
| **Severity** | Medium |
| **Status** | Fixed |
| **Protocol** | OAuth 2.0 |

---

# 🧠 Technical Explanation

The application implemented an OAuth 2.0 authorization flow.

During testing, I discovered that the authorization process lacked several important security controls:

### Identified weaknesses

- ❌ OAuth clients could be registered without sufficient restrictions.
- ❌ Redirect URI validation was not strict enough.
- ❌ PKCE was not mandatory during authorization requests.

## What is PKCE?

**PKCE (Proof Key for Code Exchange)** is an OAuth 2.0 security extension designed to prevent **authorization code interception attacks**.

It introduces a `code_verifier` and `code_challenge` mechanism, ensuring that only the legitimate client that initiated the authentication flow can exchange the authorization code for tokens.

Without PKCE enforcement, authorization codes may become vulnerable if intercepted.

---

# ⚔️ Attack Scenario

An attacker could potentially:

1. Register an OAuth client with a malicious configuration.
2. Generate a crafted OAuth authorization request.
3. Convince a victim to complete the authentication flow.
4. Attempt to capture the authorization code.
5. Exchange the code under specific attack conditions.

### Required conditions

The attack was not fully standalone and required additional factors:

- User interaction
- A suitable redirect/listener setup
- A vulnerable OAuth client registration flow

---

# 🔍 Root Cause Analysis

The vulnerability originated from multiple configuration weaknesses:

### 1. Missing PKCE Enforcement

The authorization server allowed OAuth flows without requiring:

```
code_challenge_method=S256
```

This weakened the protection against authorization code interception.

---

### 2. Weak OAuth Client Registration Controls

Client registration lacked sufficient restrictions, allowing potentially unsafe OAuth client configurations.

---

### 3. Insufficient Redirect URI Validation

Redirect URI handling did not fully prevent unsafe client-controlled destinations.

---

# 💥 Impact Assessment

Potential security impact:

| Impact | Description |
|---|---|
| 🔑 Authorization Code Exposure | Possible leakage of OAuth authorization codes |
| 👤 Account Compromise | Possible under specific exploitation conditions |
| 🔄 OAuth Abuse | Malicious OAuth client registration scenarios |

The vulnerability severity was assessed as **Medium** due to the required attack conditions.

---

# 🛠️ Remediation

Recommended security improvements:

✅ Enforce PKCE for all authorization code flows.

Example:

```
code_challenge_method=S256
```

✅ Strictly validate redirect URIs.

✅ Restrict OAuth Dynamic Client Registration.

✅ Follow modern OAuth security recommendations.

---

# 📚 References

- [RFC 7636 — Proof Key for Code Exchange (PKCE)](https://datatracker.ietf.org/doc/html/rfc7636)
- [RFC 7591 — OAuth 2.0 Dynamic Client Registration](https://datatracker.ietf.org/doc/html/rfc7591)
- [RFC 8252 — OAuth 2.0 for Native Apps](https://datatracker.ietf.org/doc/html/rfc8252)

---

# 📢 Responsible Disclosure

This vulnerability was reported through a responsible disclosure process.

The vendor acknowledged the issue and implemented the recommended security fixes.

---

## 🏷️ Tags

`OAuth2` `PKCE` `Authentication` `BugBounty` `SecurityResearch` `WebSecurity`
