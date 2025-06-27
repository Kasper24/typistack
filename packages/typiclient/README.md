# @typistack/client

TypeScript client for type-safe API calls to typistack servers.

## Features

- Type-safe client for typistack routers
- Superjson serialization
- Zod validation support
- Request/response interceptors
- Dot and bracket syntax for most routes (see below)

## Installation

```bash
npm install @typistack/client
```

## Usage

### Basic Usage

```ts
import { createTypiClient } from '@typistack/client';

const client = createTypiClient({
  baseUrl: 'http://localhost:3000',
});

// Dot syntax (type-safe if you provide router types)
const user = await client.user.me.get();

// Bracket syntax (always works)
const user2 = await client['user']['me']['get']();
```

### Route With Path Parameter (must use bracket syntax for dynamic segments)

```ts
// For a route: /post/:id (GET)
const post = await client.post[':id'].get({ input: { path: { id: '123' } } });

// Or, using full bracket notation:
const post2 = await client['post'][':id']['get']({ input: { path: { id: '123' } } });
```

### Using Client Options Per Request

```ts
const result = await client.user.me.get({
  options: { timeout: 1000, throwOnErrorStatus: true }
});
```

## API

- `createTypiClient({ baseUrl, baseHeaders?, interceptors?, options? })`: Create a new client instance.
- Dynamic proxy: Use dot or bracket notation to access routes/methods. For routes with path params, use bracket syntax and the exact segment name (e.g., ':id').
- Supports request/response interceptors and custom headers.
- Per-request options: `{ options: { timeout, throwOnErrorStatus, ... } }`

## Development

- Build: `npm run build`
- Dev: `npm run dev`
- Lint: `npm run lint:check`
- Type check: `npm run lint:types`

## License

MIT
