---
name: payload-optimization
description: API response size and serialization efficiency
category: performance
tags: [payload, api, serialization, compression]
---

# Payload Optimization

## When to Use

- Reducing bandwidth usage for high-traffic APIs
- Minimizing response times for large datasets
- Improving mobile and slow network performance

## Rules

- Return only required fields in responses
- Avoid deeply nested objects
- Avoid large lists without pagination
- Use projection queries to fetch only needed fields
- Use compression (gzip) when applicable
- Maintain stable response schema
- Avoid breaking changes in contracts

## Pattern

Bad - Over-fetching with nested data:

```java
@GetMapping("/orders/{id}")
public Order getOrder(@PathVariable Long id) {
    Order order = orderRepository.findById(id).orElseThrow();
    // Returns all customer data, all line items, all nested objects
    return order;
}
```

Good - Projection with only needed fields:

```java
@GetMapping("/orders/{id}")
public OrderSummaryDTO getOrder(@PathVariable Long id) {
    return orderRepository.findSummaryById(id)
        .map(o -> new OrderSummaryDTO(o.id, o.total, o.status))
        .orElseThrow();
}
```

## Avoid

- Exposing unnecessary relationships
- Deep nesting in response schemas
- Unpaginated collection endpoints
- Over-fetching and chatty payload design
