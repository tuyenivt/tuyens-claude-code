---
name: caching
description: Cache strategy and invalidation patterns
metadata:
  category: performance
  tags: [caching, redis, ttl, invalidation]
user-invocable: false
---

# Caching

## When to Use

- Read-heavy operations with predictable access patterns
- Expensive computations that can be reused
- Stable reference data that changes infrequently

## Rules

- Cache only DTO, never entity objects
- Define TTL explicitly for all cached entries
- Eviction strategy required before deployment
- Cache invalidation on write operations
- Avoid stale critical data
- Prevent cache stampede with proper locking

## Pattern

Bad - No TTL, caching entity:

```java
@Cacheable(value = "users")
public User getUser(Long id) {
    return userRepository.findById(id).orElseThrow();
}
```

Good - TTL with DTO:

```java
@Cacheable(value = "users", cacheManager = "cacheManagerWithTTL")
public UserDTO getUser(Long id) {
    return userMapper.toDTO(userRepository.findById(id).orElseThrow());
}
```

## Avoid

- Caching highly volatile data
- Missing eviction strategies
- Caching entity objects directly
- Cache stampede without mitigation
