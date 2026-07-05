# RTK Query Example

Server data with caching and tag invalidation. Maps to [14-rtk-query.md](../../docs/14-rtk-query.md) and [diagrams/03-rtk-query-flow.md](../../diagrams/03-rtk-query-flow.md).

## Scenario

Fetch and create todos; creating one auto-refreshes the list.

## Code

```ts
// src/features/todos/api/todosApi.ts
import { baseApi } from "@/app/api/baseApi";

interface Todo {
  id: string;
  title: string;
  done: boolean;
}

export const todosApi = baseApi.injectEndpoints({
  endpoints: (build) => ({
    getTodos: build.query<Todo[], void>({
      query: () => "/todos",
      providesTags: (result) =>
        result
          ? [...result.map((t) => ({ type: "Todo" as const, id: t.id })), { type: "Todo", id: "LIST" }]
          : [{ type: "Todo", id: "LIST" }],
    }),
    addTodo: build.mutation<Todo, Pick<Todo, "title">>({
      query: (body) => ({ url: "/todos", method: "POST", body }),
      invalidatesTags: [{ type: "Todo", id: "LIST" }],
    }),
  }),
});

export const { useGetTodosQuery, useAddTodoMutation } = todosApi;
```

```tsx
// Usage in a component
const { data: todos, isLoading, isError } = useGetTodosQuery();
const [addTodo, { isLoading: isAdding }] = useAddTodoMutation();

if (isLoading) return <Spinner />;
if (isError) return <ErrorState />;
```

## Explanation

- `providesTags`/`invalidatesTags` refresh the list automatically after a mutation — no manual refetch.
- Endpoints are typed (`Result`, `Arg`); hooks are auto-generated.
- The cache is the single source of truth for server data.

## Related

[14-rtk-query.md](../../docs/14-rtk-query.md) · [api template](../../templates/api-template.md)
