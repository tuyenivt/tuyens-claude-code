---
name: task-react-page
description: Create new React page with routing and data fetching
metadata:
  category: frontend
  tags: [react, typescript, page, routing, data-fetching]
  type: composite
---

# React Page

## When to Use

- Creating new React pages with routing
- Pages with data fetching and loading states
- Next.js or React Router page scaffolding

## Implementation

Generate page with:

1. **Page component** - With data fetching
2. **Loading state** - Skeleton or spinner
3. **Error handling** - Error boundary or fallback
4. **Route config** - React Router or Next.js

## Patterns

### React Router + React Query

```tsx
import { useParams } from "react-router-dom";
import { useQuery } from "@tanstack/react-query";

export const ResourcePage: FC = () => {
  const { id } = useParams<{ id: string }>();
  const { data, isLoading, error } = useQuery({
    queryKey: ["resource", id],
    queryFn: () => fetchResource(id!),
  });

  if (isLoading) return <Skeleton />;
  if (error) return <ErrorDisplay error={error} />;

  return <ResourceDetails resource={data} />;
};
```

### Next.js App Router

```tsx
export default async function Page({ params }: { params: { id: string } }) {
  const resource = await fetchResource(params.id);
  if (!resource) notFound();
  return <ResourceDetails resource={resource} />;
}
```

## Key Skills Reference

For detailed patterns, reference these skills:

- Use skill: `react-component-design` for page structure and composition
- Use skill: `react-state-management` for data fetching and state organization
- Use skill: `payload-optimization` for code splitting and lazy loading

## Rules

- Always handle loading, error, and empty states
- Use skeletons over spinners when layout is predictable
- Include SEO metadata
- Ensure responsive layout
- Support keyboard navigation

## Checklist

- [ ] Loading states with skeletons
- [ ] Error handling with recovery
- [ ] SEO metadata
- [ ] Responsive layout
- [ ] Keyboard navigation

## Avoid

- Pages without loading states
- Unhandled error scenarios
- Missing SEO metadata
- Non-responsive layouts
