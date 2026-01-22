# Security Policy

## Purpose

This policy defines how `tuyens-claude-code` handles sensitive data and enforces security practices. These rules treat AI as an untrusted system and enforce defense in depth.

---

## Core Security Principles

1. **AI is untrusted** - Claude output should be treated as untrusted input requiring validation
2. **No secrets ever** - Secrets, credentials, and keys must never be shared with AI
3. **Minimize data exposure** - Share only the minimum code necessary for analysis
4. **Verify everything** - AI analysis must be verified by humans before action
5. **Audit trail** - Maintain records of AI-assisted decisions

---

## Data Classification

### NEVER Share with AI

These data types must NEVER be included in prompts:

| Category            | Examples                                      |
| ------------------- | --------------------------------------------- |
| **Credentials**     | Passwords, API keys, tokens, private keys     |
| **Secrets**         | Encryption keys, signing certificates         |
| **PII**             | Names, emails, addresses, SSNs in actual data |
| **PHI**             | Health records, medical information           |
| **Financial**       | Credit cards, bank accounts, transaction data |
| **Production Data** | Real customer data, even "anonymized"         |

### Safe to Share with AI

These can be shared with appropriate care:

| Category                  | Guidance                               |
| ------------------------- | -------------------------------------- |
| **Source Code**           | Remove hardcoded secrets first         |
| **Schema Definitions**    | Tables and columns, not data           |
| **Configuration**         | Structure, not values                  |
| **Error Messages**        | Stack traces without sensitive context |
| **Architecture Diagrams** | Logical structure, not credentials     |

---

## Secret Detection

### If Secrets Are Detected

When Claude detects potential secrets in shared code:

1. **STOP** - Halt analysis immediately
2. **WARN** - Alert that secrets were detected
3. **DO NOT DISPLAY** - Never repeat the secret in output
4. **RECOMMEND** - Advise immediate rotation
5. **REFUSE** - Do not proceed until secrets are removed

### Secret Patterns to Watch For

Claude will flag patterns that may indicate secrets:

- API keys: `AKIA...`, `sk-...`, `Bearer ...`
- Connection strings with credentials
- Private keys: `-----BEGIN RSA PRIVATE KEY-----`
- AWS credentials, GCP keys, Azure secrets
- Database passwords in configuration
- JWT secrets

---

## Redaction Requirements

### Before Sharing Code

Users SHOULD redact:

```java
// BAD - Contains actual credentials
spring.datasource.password=actual_password_here

// GOOD - Redacted
spring.datasource.password=<REDACTED>

// BAD - Contains real API key
private static final String API_KEY = "sk-abc123...";

// GOOD - Placeholder
private static final String API_KEY = "<API_KEY>";
```

### In Code Examples

Claude MUST use placeholders:

```java
// Always use placeholders in examples
String apiKey = System.getenv("API_KEY");  // Good
String password = "<REDACTED>";             // Good
String dbUrl = "jdbc:postgresql://host:5432/db";  // Good (no creds)
```

---

## Security Review Limitations

### What Security Reviews CAN Do

- Identify common vulnerability patterns
- Flag suspicious code constructs
- Reference OWASP guidelines
- Suggest areas for security testing
- Note missing security controls

### What Security Reviews CANNOT Do

- Certify code as "secure"
- Replace penetration testing
- Validate compliance with regulations
- Detect all vulnerabilities
- Guarantee protection against attacks

---

## AI Output as Untrusted Input

### Treat AI Suggestions Like External Input

When Claude provides suggestions:

1. **Validate** - Verify suggestions match your codebase
2. **Test** - Never deploy AI-suggested changes without testing
3. **Review** - Have humans review before implementation
4. **Audit** - Log that AI assistance was used

### Do Not Directly Execute

Never:

- Copy-paste AI output directly to production
- Execute AI-generated commands without review
- Use AI output in security-critical paths without validation
- Assume AI output is correct

---

## Incident Response

### If Secrets Were Accidentally Shared

1. **Rotate immediately** - Assume the secret is compromised
2. **Revoke access** - Disable the compromised credential
3. **Audit usage** - Check for unauthorized access
4. **Document** - Record the incident
5. **Prevent recurrence** - Improve processes

### If AI Output Causes a Security Issue

1. **Contain** - Limit the impact
2. **Document** - Record what AI suggested and what was implemented
3. **Analyze** - Understand why human review didn't catch the issue
4. **Improve** - Update review processes

---

## Compliance Considerations

### AI-Assisted Development and Compliance

When compliance requirements apply:

- **Document AI usage** - Note where AI assisted development
- **Human accountability** - Humans remain responsible for decisions
- **Validate AI output** - Compliance cannot be delegated to AI
- **Audit trail** - Maintain records of AI-assisted code changes

### Compliance Claude Cannot Provide

- SOC2 certification
- HIPAA compliance attestation
- PCI-DSS validation
- GDPR compliance confirmation
- Any regulatory certification

---

## Network and Access

### Claude Code Plugin Access

This plugin:

- Does NOT store conversation history
- Does NOT have access to your file system beyond what you share
- Does NOT make network calls on your behalf
- Does NOT persist data between sessions

### Safe Usage Patterns

```bash
# Good - Review specific files
/security-review src/main/java/com/example/auth/

# Good - Clear scope
/jpa-review --entity Order --concern performance

# Bad - Too broad, may include secrets
/security-review --all
```

---

## Security Contacts

For security concerns with this plugin:

- File an issue on the GitHub repository
- Mark security issues as confidential
- Do not include sensitive data in issue reports

---

## Version

Policy Version: 1.0.0
Last Updated: 202601
