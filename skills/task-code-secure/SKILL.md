---
name: task-code-secure
description: Security review covering OWASP Top 10, auth, and common vulnerabilities
metadata:
  category: review
  tags: [security, owasp, vulnerabilities, auth]
  type: workflow
---

# Code Secure

## When to Use

- Security reviews of code changes
- Pre-deployment security checks
- Vulnerability assessment
- Authentication and authorization review

## Security Checklist (Framework-Aware)

### Injection

- [ ] Parameterized queries (no SQL concatenation)
- [ ] Input validation on all user inputs
- [ ] Output encoding (prevent XSS)
- [ ] No command injection vulnerabilities
- [ ] No template injection

### Authentication

**Backend (Java/Spring, Python/Django, etc.):**

- [ ] Endpoints secured (framework-specific decorators/annotations)
- [ ] Password hashing (BCrypt, Argon2)
- [ ] JWT/Session validation (signature, expiry)
- [ ] No credentials in code

**Frontend (React, Vue, etc.):**

- [ ] Tokens stored securely (httpOnly cookies, not localStorage)
- [ ] CSRF protection enabled
- [ ] No sensitive data in client-side state

### Authorization

- [ ] Role-based access control (RBAC)
- [ ] Resource ownership validated
- [ ] No privilege escalation paths
- [ ] Principle of least privilege

### Data Protection

- [ ] Sensitive data not logged
- [ ] Encryption for sensitive fields (PII, passwords)
- [ ] Secure headers (CORS, CSP, HSTS)
- [ ] No secrets in environment variables (use secret manager)

## OWASP Quick Check

| Risk                      | Check                 |
| ------------------------- | --------------------- |
| Broken Access Control     | Auth on all endpoints |
| Injection                 | Parameterized queries |
| Cryptographic Failures    | No weak algorithms    |
| Security Misconfiguration | Secure headers        |

## Key Skills Reference

For security patterns, reference these skills:

- Use skill: `exception-handling` for secure error responses (no stack traces)
- Use skill: `observability` for logging without sensitive data
- Use skill: `resiliency` for secure and resilient external API communication
- Use skill: `idempotency` for safe retry patterns

## Rules

- Always validate at system boundaries (user input, external APIs)
- Never trust client-side data
- Log security events without sensitive data
- Follow principle of least privilege

## Output

```markdown
## Summary

[Security posture assessment]

## Findings by Severity

### Critical | High | Medium | Low

- **Location:** [file:line]
- **Issue:** [vulnerability]
- **Impact:** [attack scenario]

## Recommendations

[Prioritized security improvements]
```

## Avoid

- Ignoring framework-specific security features
- Storing secrets in code or environment variables
- Disabling security features for convenience
- Logging sensitive data (passwords, tokens, PII)
