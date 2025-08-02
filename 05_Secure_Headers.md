# 5. Secure Headers

HTTP security headers are a simple yet powerful way to instruct the browser to enable security features, protecting your users from certain types of attacks like **XSS** and **clickjacking**.

## 📋 Secure Headers Checklist

### [ ] Strict-Transport-Security (HSTS)
**Enforces the use of HTTPS** and prevents downgrade attacks.

```
Strict-Transport-Security: max-age=31536000; includeSubDomains
```

### [ ] Content-Security-Policy (CSP)
Controls which resources (scripts, styles, images) a browser is allowed to load. This is a **highly effective defense against XSS**. A restrictive policy is best:

```
Content-Security-Policy: default-src 'self';
```

### [ ] X-Content-Type-Options
Prevents the browser from **MIME-sniffing** a response away from the declared content-type.

```
X-Content-Type-Options: nosniff
```

### [ ] X-Frame-Options
Protects against **clickjacking** by controlling whether your API responses can be embedded in `<frame>` or `<iframe>` elements.

```
X-Frame-Options: DENY
```
or
```
X-Frame-Options: SAMEORIGIN
```

### [ ] Cross-Origin-Resource-Policy (CORP) and Cross-Origin-Opener-Policy (COOP)
Help mitigate **speculative execution side-channel attacks** like Spectre.

```
Cross-Origin-Resource-Policy: same-origin
Cross-Origin-Opener-Policy: same-origin
```

### [ ] Configure Cross-Origin Resource Sharing (CORS) Correctly
**Do not use wildcards (`*`)** for `Access-Control-Allow-Origin` in production. Explicitly list the domains that are allowed to access your API.

## 🚨 Common Header Vulnerabilities

* **Missing security headers** leaving applications vulnerable to common attacks
* **Overly permissive CSP policies** that don't effectively prevent XSS
* **Wildcard CORS configuration** allowing any domain to access the API
* **Missing HSTS** allowing HTTPS downgrade attacks
* **Incorrect header values** that don't provide the intended protection

## 🔍 Header Best Practices

* **Test headers with security tools** like OWASP ZAP or Burp Suite
* **Use security header testing services** to verify implementation
* **Monitor header effectiveness** through security monitoring
* **Regularly update header policies** as new threats emerge
* **Document header configurations** for team reference

## 📊 Complete Security Headers Example

```http
HTTP/1.1 200 OK
Content-Type: application/json
Strict-Transport-Security: max-age=31536000; includeSubDomains
Content-Security-Policy: default-src 'self'; script-src 'self' 'unsafe-inline'; style-src 'self' 'unsafe-inline'
X-Content-Type-Options: nosniff
X-Frame-Options: DENY
Cross-Origin-Resource-Policy: same-origin
Cross-Origin-Opener-Policy: same-origin
X-XSS-Protection: 1; mode=block
Referrer-Policy: strict-origin-when-cross-origin
Permissions-Policy: geolocation=(), microphone=(), camera=()
```

## 🔧 CORS Configuration Example

```javascript
// Good CORS configuration
app.use(cors({
  origin: ['https://yourdomain.com', 'https://app.yourdomain.com'],
  methods: ['GET', 'POST', 'PUT', 'DELETE'],
  allowedHeaders: ['Content-Type', 'Authorization'],
  credentials: true,
  maxAge: 86400
}));

// Bad CORS configuration (DON'T DO THIS)
app.use(cors({
  origin: '*', // This allows any domain!
  credentials: true
}));
```

## 🛡️ Additional Security Headers

Consider implementing these additional headers for enhanced security:

* **X-XSS-Protection:** Additional XSS protection for older browsers
* **Referrer-Policy:** Control referrer information in requests
* **Permissions-Policy:** Control browser feature access
* **Cache-Control:** Prevent sensitive data caching

---

*Next: Learn about [Logging & Monitoring](06_Logging_and_Monitoring.md) to detect and respond to attacks.*