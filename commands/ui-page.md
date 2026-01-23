---
description: Create new React page with routing and data fetching
model: claude-sonnet-4-5
---

Create a new React page with routing, data fetching, and layout.

## Requirements

$ARGUMENTS

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

## Checklist

- Loading states with skeletons
- Error handling with recovery
- SEO metadata
- Responsive layout
- Keyboard navigation
