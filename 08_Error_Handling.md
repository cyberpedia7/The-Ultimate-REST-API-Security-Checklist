# 8. Secure Error Handling

Error messages can be a **goldmine for attackers**, revealing information about the underlying technology stack, database schemas, or internal application logic.

## ⚠️ Secure Error Handling Checklist

### [ ] Implement a Global Error Handler
Catch all unhandled exceptions to prevent them from propagating to the client.

### [ ] Return Generic Error Messages
**Do not send detailed error messages or stack traces to the client** in a production environment.

**Bad:**
```json
{
  "error": "ERROR: column 'user_password' does not exist in table 'users'"
}
```

**Good:**
```json
{
  "status": 500,
  "message": "An internal server error occurred."
}
```

### [ ] Log Detailed Error Information Server-Side
While the client gets a generic message, you should **log the full, detailed error** (including the stack trace) on the server for debugging purposes.

### [ ] Use Appropriate HTTP Status Codes
Use standard codes to indicate the nature of the error:
* `400 Bad Request` for validation errors
* `401 Unauthorized` for authentication issues
* `403 Forbidden` for authorization failures
* `404 Not Found` for missing resources
* `429 Too Many Requests` for rate limiting
* `500 Internal Server Error` for server-side issues

## 🚨 Common Error Handling Vulnerabilities

* **Detailed error messages** revealing system internals
* **Stack traces exposed** to clients
* **Inconsistent error responses** across endpoints
* **Missing error logging** making debugging difficult
* **Inappropriate status codes** confusing clients

## 🔍 Error Handling Best Practices

* **Use consistent error response format** across all endpoints
* **Include correlation IDs** for error tracking
* **Log errors with appropriate context** (user ID, request details)
* **Implement error monitoring** to detect patterns
* **Regularly review error messages** for information leakage

## 📝 Example Error Response Format

### Standard Error Response
```json
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Invalid input provided",
    "correlation_id": "req-12345-abcde",
    "timestamp": "2024-01-15T10:30:45.123Z"
  }
}
```

### Validation Error Example
```json
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Request validation failed",
    "details": [
      {
        "field": "email",
        "message": "Invalid email format"
      },
      {
        "field": "password",
        "message": "Password must be at least 8 characters"
      }
    ],
    "correlation_id": "req-12345-abcde"
  }
}
```

## 🔧 Global Error Handler Example

### Express.js Error Handler
```javascript
app.use((error, req, res, next) => {
  // Log the full error for debugging
  console.error('Error:', {
    message: error.message,
    stack: error.stack,
    url: req.url,
    method: req.method,
    user_id: req.user?.id,
    correlation_id: req.correlation_id
  });

  // Determine appropriate status code
  let statusCode = 500;
  let errorCode = 'INTERNAL_ERROR';
  let message = 'An internal server error occurred';

  if (error.name === 'ValidationError') {
    statusCode = 400;
    errorCode = 'VALIDATION_ERROR';
    message = 'Invalid input provided';
  } else if (error.name === 'UnauthorizedError') {
    statusCode = 401;
    errorCode = 'UNAUTHORIZED';
    message = 'Authentication required';
  }

  // Return generic error to client
  res.status(statusCode).json({
    error: {
      code: errorCode,
      message: message,
      correlation_id: req.correlation_id,
      timestamp: new Date().toISOString()
    }
  });
});
```

## 🛡️ Error Information Classification

### Safe to Expose
* Generic error messages
* Validation error details (field names and rules)
* Rate limit information
* Authentication status

### Never Expose
* Database connection details
* File system paths
* Internal API endpoints
* Stack traces
* Configuration values
* User data in error messages

## 📊 Error Monitoring Metrics

* **Error rates** by endpoint and error type
* **Response time** for error handling
* **Correlation** between errors and user actions
* **Geographic distribution** of errors
* **Trends** in error patterns over time

---

*Next: Review the complete [Checklist Summary](09_Checklist_Summary.md) for a comprehensive security overview.*