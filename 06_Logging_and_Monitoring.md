# 6. Logging and Monitoring

You cannot defend against what you cannot see. **Robust logging and monitoring are critical** for detecting attacks in progress and for forensic analysis after an incident.

## 📊 Logging and Monitoring Checklist

### [ ] Log All Important Events
This includes:

* **Every API request and response** (including URL, verb, status code, IP address, user agent)
* **Authentication successes and failures**
* **Authorization failures** (denied access attempts)
* **Server-side errors**
* **Changes to sensitive data**

### [ ] Do Not Log Sensitive Information
**Never log passwords, API keys, session tokens, or personally identifiable information (PII)** in plain text.

### [ ] Use a Standardized Log Format
Formats like **JSON** make logs much easier to parse, search, and analyze by automated systems.

### [ ] Centralize Your Logs
Ship logs from all your services to a **central logging system** (e.g., ELK Stack, Splunk, Datadog).

### [ ] Implement Monitoring and Alerting
Create dashboards to visualize key security metrics (e.g., 4xx/5xx error rates, login failures).

Set up **automated alerts** for suspicious events, such as:

* A high rate of failed logins from a single IP
* A sudden spike in 403 Forbidden errors
* Attempts to access non-existent endpoints (potential scanning activity)
* A single user making an unusually high number of requests

## 🚨 Common Logging Vulnerabilities

* **Missing or insufficient logging** making incident response difficult
* **Logging sensitive data** in plain text
* **Inconsistent log formats** making analysis difficult
* **No centralized logging** making correlation impossible
* **Missing monitoring and alerting** allowing attacks to go unnoticed

## 🔍 Logging Best Practices

* **Use structured logging** with consistent field names
* **Include correlation IDs** to trace requests across services
* **Set appropriate log levels** (DEBUG, INFO, WARN, ERROR)
* **Implement log rotation** to manage storage
* **Regularly review and update** logging policies

## 📝 Example Log Entry

```json
{
  "timestamp": "2024-01-15T10:30:45.123Z",
  "level": "INFO",
  "correlation_id": "req-12345-abcde",
  "event": "api_request",
  "method": "POST",
  "url": "/api/users",
  "status_code": 201,
  "ip_address": "192.168.1.100",
  "user_agent": "Mozilla/5.0...",
  "user_id": "user-123",
  "response_time_ms": 245,
  "request_size_bytes": 1024
}
```

## 🚨 Security Alert Examples

### Failed Login Alert
```json
{
  "alert_type": "failed_login_spike",
  "ip_address": "192.168.1.100",
  "failed_attempts": 15,
  "time_window": "5 minutes",
  "threshold": 10,
  "timestamp": "2024-01-15T10:30:45.123Z"
}
```

### Unauthorized Access Alert
```json
{
  "alert_type": "unauthorized_access",
  "endpoint": "/api/admin/users",
  "user_id": "user-123",
  "ip_address": "192.168.1.100",
  "timestamp": "2024-01-15T10:30:45.123Z"
}
```

## 📊 Monitoring Dashboard Metrics

* **Request Volume:** Total requests per minute/hour
* **Error Rates:** 4xx and 5xx error percentages
* **Authentication Failures:** Failed login attempts
* **Authorization Failures:** 403 Forbidden responses
* **Response Times:** Average and percentile response times
* **Rate Limit Violations:** 429 responses
* **Geographic Distribution:** Requests by country/region

## 🔧 Logging Configuration Example

```yaml
logging:
  level: INFO
  format: json
  fields:
    service: api-gateway
    environment: production
    version: 1.0.0
  
  sensitive_fields:
    - password
    - api_key
    - token
    - credit_card
    - ssn
  
  retention:
    days: 90
    max_size: "10GB"
  
  destinations:
    - console
    - file: /var/log/api.log
    - syslog: udp://log-server:514
```

---

*Next: Learn about [Secrets Management](07_Secrets_Management.md) to securely handle credentials and keys.*