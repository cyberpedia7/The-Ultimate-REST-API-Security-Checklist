# 7. Secrets Management

"Secrets" include API keys, database credentials, encryption keys, and anything else that grants access to a system. They must be handled with **extreme care**.

## 🔑 Secrets Management Checklist

### [ ] Never Hardcode Secrets in Source Code
This is one of the most common and dangerous mistakes. **Secrets in code are a major security risk**.

### [ ] Do Not Store Secrets in Configuration Files
While better than hardcoding, config files are often checked into version control or are otherwise insecurely handled.

### [ ] Use a Dedicated Secrets Management Solution
Choose from established solutions:

* **Cloud-based:**
  * AWS Secrets Manager
  * Azure Key Vault
  * Google Cloud Secret Manager
* **Self-hosted:**
  * HashiCorp Vault

These tools provide **secure storage, fine-grained access control, auditing, and key rotation capabilities**.

### [ ] Inject Secrets at Runtime
The application should retrieve its secrets from the secrets manager when it starts up, not at build time.

### [ ] Implement Key Rotation
Regularly rotate all credentials and keys to limit the window of opportunity for an attacker if a key is compromised. **Automate this process** wherever possible.

### [ ] Apply the Principle of Least Privilege
An application should only have access to the secrets it absolutely needs.

## 🚨 Common Secrets Management Vulnerabilities

* **Hardcoded secrets** in source code or configuration files
* **Secrets in version control** history
* **Overly permissive access** to secrets
* **No key rotation** allowing compromised keys to remain valid
* **Insecure secret transmission** over unencrypted channels
* **Secrets in environment variables** that may be logged

## 🔍 Secrets Management Best Practices

* **Use environment-specific secrets** for different deployment environments
* **Implement secret versioning** to track changes
* **Monitor secret access** for unusual patterns
* **Use temporary credentials** where possible
* **Regularly audit secret permissions** and remove unnecessary access

## 📝 Example Secrets Management Implementation

### AWS Secrets Manager Example
```javascript
const AWS = require('aws-sdk');
const secretsManager = new AWS.SecretsManager();

async function getDatabaseCredentials() {
  try {
    const data = await secretsManager.getSecretValue({
      SecretId: 'production/database-credentials'
    }).promise();
    
    return JSON.parse(data.SecretString);
  } catch (error) {
    console.error('Error retrieving secret:', error);
    throw error;
  }
}
```

### HashiCorp Vault Example
```javascript
const vault = require('node-vault');

const client = vault({
  apiVersion: 'v1',
  endpoint: 'https://vault.example.com:8200',
  token: process.env.VAULT_TOKEN
});

async function getSecret(path) {
  try {
    const result = await client.read(`secret/data/${path}`);
    return result.data.data;
  } catch (error) {
    console.error('Error retrieving secret:', error);
    throw error;
  }
}
```

## 🔄 Key Rotation Strategy

### Automated Rotation
```yaml
rotation_schedule:
  database_passwords: 90 days
  api_keys: 180 days
  ssl_certificates: 365 days
  encryption_keys: 365 days

rotation_process:
  - Generate new secret
  - Update in secrets manager
  - Deploy to staging environment
  - Test functionality
  - Deploy to production
  - Remove old secret
  - Update monitoring
```

## 🛡️ Secrets Security Checklist

* [ ] **No secrets in code** - All secrets are externalized
* [ ] **Encrypted storage** - Secrets are encrypted at rest
* [ ] **Access logging** - All secret access is logged
* [ ] **Regular rotation** - Keys are rotated on schedule
* [ ] **Least privilege** - Minimal access to secrets
* [ ] **Secure transmission** - Secrets transmitted over encrypted channels
* [ ] **Backup strategy** - Secrets are backed up securely
* [ ] **Incident response** - Plan for compromised secrets

## 📊 Secrets Inventory

Maintain an inventory of all secrets:

| Secret Type | Location | Rotation Schedule | Access Control |
|-------------|----------|------------------|----------------|
| Database Password | AWS Secrets Manager | 90 days | Application role only |
| API Keys | HashiCorp Vault | 180 days | Service-specific roles |
| SSL Certificates | Certificate Manager | 365 days | DevOps team |
| Encryption Keys | Hardware Security Module | 365 days | Security team |

---

*Next: Learn about [Error Handling](08_Error_Handling.md) to avoid information leakage.*