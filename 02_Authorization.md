# 2. Authorization

Authorization happens after authentication. It determines what an authenticated user is allowed to do. This is where many critical vulnerabilities, like **Broken Object Level Authorization (BOLA)**, occur.

## 🛡️ Authorization Checklist

### [ ] Never Trust Client Data for Authorization
**Critical:** Never trust data from the client for authorization decisions. For example:
* Do not trust a `role: "admin"` claim in a JWT without verifying its signature
* Do not trust an incoming `userID` from a URL or request body to determine ownership

### [ ] Enforce the Principle of Least Privilege
Users and services should only have the **absolute minimum permissions** required to perform their functions.

### [ ] Implement Robust Access Control on Every Request
Every API request must be validated for proper authorization before processing.

### [ ] Verify Resource Ownership
For any request that accesses a resource (e.g., `GET /api/users/{id}` or `PUT /api/orders/{orderId}`), verify that the authenticated user has the right to access that specific resource.

**Example:** A user with `id=123` should not be able to access `GET /api/users/456/profile`. The check must be: *"Is the logged-in user's ID equal to the ID in the URL?"*

### [ ] Use Centralized Authorization
Implement access control logic in a **single, well-tested place** (e.g., middleware, API gateway) rather than scattering it throughout the application code.

### [ ] Implement RBAC or ABAC
Choose the appropriate access control model:

* **RBAC (Role-Based Access Control):** Assign users to roles (e.g., admin, editor, viewer) and grant permissions to those roles
* **ABAC (Attribute-Based Access Control):** Use attributes of the user, resource, and environment to make more granular access decisions

### [ ] Filter Collection Results
For APIs that expose collections, filter results based on user permissions. An endpoint like `GET /api/documents` should only return documents that the requesting user is allowed to see.

## 🚨 Common Authorization Vulnerabilities

* **BOLA (Broken Object Level Authorization):** Users can access resources they shouldn't
* **Trusting client-side role claims** without server-side verification
* **Missing authorization checks** on sensitive endpoints
* **Overly permissive access controls** that violate least privilege
* **Inconsistent authorization logic** scattered throughout the codebase

## 🔍 Authorization Best Practices

* **Always verify on the server side** - never trust client assertions
* **Test with different user roles** to ensure proper access control
* **Log authorization failures** for security monitoring
* **Use consistent authorization patterns** across your API
* **Regularly audit permissions** and remove unnecessary access

---

*Next: Learn about [Data Validation](03_Data_Validation.md) to prevent malicious input.*