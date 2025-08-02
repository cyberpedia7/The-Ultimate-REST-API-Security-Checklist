# 1. Authentication

Authentication is the process of verifying a client's identity. Anonymous access should be disabled unless explicitly required.

## 🔐 Authentication Checklist

### [ ] Use Standard Authentication Protocols
**Never invent your own authentication system.** Use well-vetted, industry-standard protocols:

* **OAuth 2.0 / OIDC:** The industry standard for delegated authorization. Ideal for third-party client access and single sign-on (SSO).
* **JSON Web Tokens (JWTs):** A popular choice for stateless authentication between services.

### [ ] Enforce Strong Password Policies
If you are managing user credentials directly, follow **NIST guidelines**:
* Minimum length requirements
* Complexity checks
* Checking against breached password lists
* Regular password rotation policies

### [ ] Secure JWT Implementation
When using JWTs, ensure proper configuration:

* **Use a strong, non-guessable secret key** (at least 256 bits)
* **Use the correct algorithm:** Avoid `none`. Prefer RS256 (asymmetric) over HS256 (symmetric) if the token is verified by multiple services
* **Set a short expiration time** (`exp` claim) for tokens (e.g., 15 minutes)
* **Implement a refresh token flow** to allow for longer user sessions without exposing long-lived access tokens
* **Include claims to prevent misuse** (e.g., `aud` for audience, `iss` for issuer)

### [ ] Implement Brute-Force Protection
Protect your authentication endpoints from automated attacks:

* **Lock accounts** after a certain number of failed login attempts
* **Use rate limiting** based on IP address and/or username
* **Implement progressive delays** for repeated failed attempts

### [ ] Require Multi-Factor Authentication (MFA)
**This is the single most effective control to prevent account takeover.** Require MFA for all users, especially those with privileged access.

## 🚨 Common Authentication Pitfalls

* **Weak password policies** that don't follow NIST guidelines
* **Insecure JWT configuration** with weak secrets or incorrect algorithms
* **No brute-force protection** on login endpoints
* **Missing MFA** for sensitive accounts
* **Custom authentication systems** that haven't been properly vetted

---

*Next: Learn about [Authorization](02_Authorization.md) to control what authenticated users can access.*