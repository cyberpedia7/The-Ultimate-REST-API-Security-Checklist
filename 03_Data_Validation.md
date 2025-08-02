# 3. Data Validation

Always treat input from the client as **untrusted**. Proper data validation is your first line of defense against a wide range of injection attacks.

## ✅ Data Validation Checklist

### [ ] Use Schema-Based Validation
Define a **strict schema** for every API request body, query parameter, and URL parameter. Tools like JSON Schema are excellent for this.

### [ ] Validate Type, Format, Length, and Range
Implement comprehensive validation for all input fields:

* **Type:** If you expect an integer, reject strings
* **Format:** If you expect a date in `YYYY-MM-DD` format, reject other formats
* **Length:** If a username should be no more than 50 characters, enforce it
* **Range:** If a quantity must be between 1 and 100, reject values outside this range

### [ ] Practice Allow-Listing, Not Deny-Listing
**Only accept known good values.** For example, if a parameter can only be `active`, `inactive`, or `pending`, reject any other value.

### [ ] Sanitize and Encode Output
To prevent **Cross-Site Scripting (XSS)**, ensure that any data returned by the API that might be rendered in a browser is properly encoded (e.g., HTML entity encoding).

### [ ] Prevent Mass Assignment Vulnerabilities
Do not blindly bind incoming data to internal objects. **Explicitly map only the fields** that are supposed to be updated by the client. For example, a user should not be able to update their `isAdmin` property via a profile update endpoint.

### [ ] Use Parameterized Queries
This is the most effective way to prevent **SQL and NoSQL injection attacks**. Never build queries by concatenating strings with user input.

## 🚨 Common Validation Vulnerabilities

* **Missing input validation** allowing malicious data to enter the system
* **Insufficient validation** that doesn't check all required aspects
* **Deny-listing instead of allow-listing** which can be bypassed
* **Mass assignment vulnerabilities** allowing unauthorized field updates
* **SQL injection** through string concatenation
* **XSS through unencoded output**

## 🔍 Validation Best Practices

* **Validate on both client and server side** - never trust client-side validation alone
* **Use established validation libraries** rather than custom validation logic
* **Test with edge cases** including null values, empty strings, and boundary conditions
* **Log validation failures** for security monitoring
* **Regularly update validation rules** as requirements change

## 📝 Example Validation Schema

```json
{
  "type": "object",
  "properties": {
    "username": {
      "type": "string",
      "minLength": 3,
      "maxLength": 50,
      "pattern": "^[a-zA-Z0-9_]+$"
    },
    "email": {
      "type": "string",
      "format": "email",
      "maxLength": 254
    },
    "age": {
      "type": "integer",
      "minimum": 13,
      "maximum": 120
    }
  },
  "required": ["username", "email"]
}
```

---

*Next: Learn about [Rate Limiting & Throttling](04_Rate_Limiting_and_Throttling.md) to protect against abuse.*