# @typistack/server

TypeScript server utilities for building type-safe APIs with Express.

## Features

- Express router generator with type inference
- Zod validation for inputs
- Superjson serialization
- Middleware and error handling
- Two ways to define routes: declarative (object) or builder (imperative) style

## Installation

```bash
npm install @typistack/server
```

## Usage

### 1. Object Syntax (Declarative)

```ts
import { createTypiRouter, createTypiRoute, createTypiRouteHandler } from '@typistack/server';
import { z } from 'zod';

const authMiddleware = async (ctx) => {
  if (!ctx.req.headers.authorization) {
    return ctx.error('UNAUTHORIZED', 'Missing auth header');
  }
  // You can add more logic here
  return ctx.success(null);
};

const router = createTypiRouter({
  user: createTypiRoute({
    me: {
      get: createTypiRouteHandler({
        input: null,
        handler: async (ctx) => ctx.success({ id: '1', name: 'Alice' }),
        middlewares: [authMiddleware],
      }),
    },
  }),
  post: createTypiRoute({
    '/:id': {
      get: createTypiRouteHandler({
        input: null,
        handler: async (ctx) => {
          if (!ctx.input.path.id) return ctx.error('BAD_REQUEST', 'Missing id');
          if (ctx.input.path.id === '42') return ctx.error('NOT_FOUND', 'Post not found');
          return ctx.success({ id: ctx.input.path.id, title: 'Hello' });
        },
        middlewares: [authMiddleware],
      }),
    },
  }),
});
```

### 2. Builder Syntax (Imperative)

```ts
import { createTypiRouter, createTypiRouteHandler } from '@typistack/server';
import { z } from 'zod';

const authMiddleware = async (ctx) => {
  if (!ctx.req.headers.authorization) {
    return ctx.error('UNAUTHORIZED', 'Missing auth header');
  }
  return ctx.success({userId: 1});
};

const router = createTypiRouter();

const userRouter = createTypiRouter();
userRouter.get('me', createTypiRouteHandler({
  handler: async (ctx) => ctx.success({ id: '1', name: 'Alice' }),
  middlewares: [authMiddleware],
}));

const postRouter = createTypiRouter();
postRouter.get('/:id', createTypiRouteHandler({
  handler: async (ctx) => {
    if (!ctx.input.path.id) return ctx.error('BAD_REQUEST', 'Missing id');
    if (ctx.input.path.id === '42') return ctx.error('NOT_FOUND', 'Post not found');
    return ctx.success({ id: ctx.input.path.id, title: 'Hello' });
  },
  middlewares: [authMiddleware],
}));

router.use('user', userRouter);
router.use('post', postRouter);
```

### Mounting the Router

```ts
import express from 'express';
const app = express();
app.use('/api', router.router);
app.listen(3000);
```

## API

- `createTypiRouter(routes)`: Create a type-safe router from a route definition object.
- `createTypiRoute(obj)`: Wraps a route object for type inference (optional, but recommended for best type safety).
- `createTypiRouteHandler({ input, handler })`: Define a route handler with input validation and response typing.
- `router.use(path, subRouter)`: Mount a sub-router at a given path (builder style).

## Development

- Build: `npm run build`
- Dev: `npm run dev`
- Lint: `npm run lint:check`
- Type check: `npm run lint:types`

## License

MIT
