# 4. Rate Limiting and Throttling

Rate limiting is essential for protecting your API from **denial-of-service (DoS) attacks**, **brute-force attempts**, and general **resource exhaustion**.

## 🚦 Rate Limiting Checklist

### [ ] Implement Rate Limiting on All Endpoints
Even "read-only" endpoints can be abused to cause a DoS. **No endpoint should be exempt** from rate limiting.

### [ ] Choose the Right Limiting Strategy
Implement multiple layers of protection:

* **By IP Address:** A good baseline, but can be bypassed by distributed attacks
* **By API Key / User Account:** More effective. This ensures that one user cannot monopolize resources

### [ ] Implement Different Tiers of Rate Limiting
Set appropriate limits based on endpoint sensitivity:

* **Stricter limits** on sensitive or expensive endpoints (e.g., login, search, payment processing)
* **More lenient limits** on less critical endpoints

### [ ] Return Proper Rate Limit Responses
When a limit is exceeded, provide clear and informative responses:

* **Use the `429 Too Many Requests` HTTP status code**
* **Include headers like:**
  * `Retry-After` (to tell the client when they can try again)
  * `X-RateLimit-Remaining` (to inform the client of their current status)
  * `X-RateLimit-Limit` (to show the total limit)
  * `X-RateLimit-Reset` (to show when the limit resets)

### [ ] Implement Throttling for Resource-Intensive Operations
For tasks that consume significant CPU or memory, consider **queuing them and processing them asynchronously** to protect the real-time performance of your API.

### [ ] Set Limits on Request Payload Size
Prevent attackers from sending huge request bodies to exhaust server memory. This can typically be configured at the web server or API gateway level.

## 🚨 Common Rate Limiting Vulnerabilities

* **Missing rate limiting** on critical endpoints
* **Insufficient limits** that don't prevent abuse
* **No user-based limiting** allowing single users to monopolize resources
* **Poor error responses** that don't inform clients about limits
* **No payload size limits** allowing memory exhaustion attacks

## 🔍 Rate Limiting Best Practices

* **Monitor rate limiting effectiveness** and adjust limits based on usage patterns
* **Use sliding window algorithms** for more accurate rate limiting
* **Implement progressive delays** for repeated violations
* **Log rate limit violations** for security monitoring
* **Consider geographic rate limiting** for global APIs

## 📊 Example Rate Limit Configuration

```yaml
rate_limits:
  authentication:
    requests_per_minute: 5
    burst_limit: 10
  api_endpoints:
    requests_per_minute: 100
    burst_limit: 200
  search_endpoints:
    requests_per_minute: 30
    burst_limit: 50
  file_upload:
    requests_per_minute: 10
    max_payload_size: "10MB"
```

## 🔧 Rate Limiting Headers Example

```
HTTP/1.1 429 Too Many Requests
Retry-After: 60
X-RateLimit-Limit: 100
X-RateLimit-Remaining: 0
X-RateLimit-Reset: 1640995200
Content-Type: application/json

{
  "error": "rate_limit_exceeded",
  "message": "Too many requests. Please try again in 60 seconds.",
  "retry_after": 60
}
```

---

*Next: Learn about [Secure Headers](05_Secure_Headers.md) to enhance browser security.*