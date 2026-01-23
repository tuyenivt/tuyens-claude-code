---
description: Security review for Spring Boot APIs - OWASP, auth, vulnerabilities
model: claude-sonnet-4-5
---

Security review for Spring Boot APIs covering OWASP Top 10 and common vulnerabilities.

## Code to Review

$ARGUMENTS

## Security Checklist

### Injection

- [ ] Parameterized queries (no SQL concatenation)
- [ ] Input validation on all endpoints
- [ ] Output encoding

### Authentication

- [ ] Endpoints secured (@PreAuthorize, @Secured)
- [ ] Password hashing (BCrypt)
- [ ] JWT validation (signature, expiry)

### Authorization

- [ ] Role-based access control
- [ ] Resource ownership validated
- [ ] No privilege escalation

### Data Protection

- [ ] Sensitive data not logged
- [ ] Encryption for sensitive fields
- [ ] Secure headers (CORS, CSP)

## OWASP Quick Check

| Risk                      | Check                 |
| ------------------------- | --------------------- |
| Broken Access Control     | Auth on all endpoints |
| Injection                 | Parameterized queries |
| Cryptographic Failures    | No weak algorithms    |
| Security Misconfiguration | Secure headers        |

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
