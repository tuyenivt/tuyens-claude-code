---
description: Security review - OWASP Top 10, auth, vulnerabilities
model: claude-sonnet-4-5
---

Security review covering OWASP Top 10 and common vulnerabilities for any framework.

## Code to Review

$ARGUMENTS

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
- Use skill: `rest-integration` for secure external API communication
- Use skill: `idempotency` for safe retry patterns

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
