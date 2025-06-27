# typistack

A monorepo for modular TypeScript backend and client packages.

## Packages

- **@typistack/client**: TypeScript client SDK for interacting with typistack servers. Includes type-safe utilities and helpers for API consumption.
- **@typistack/server**: TypeScript server utilities for building robust, type-safe APIs. Includes Express middleware, Zod validation, and HTTP helpers.

## Project Structure

```text
typistack/
├── packages/
│   ├── typiclient/   # Client SDK (TypeScript)
│   │   └── src/
│   └── typiserver/   # Server utilities (TypeScript)
│       └── src/
├── package.json      # Monorepo root config (npm workspaces)
├── tsconfig.json     # Shared TypeScript config
└── .github/
    └── workflows/    # CI/CD pipelines
```

## Development

- Install dependencies: `npm install`
- Build all packages: `npm run build`
- Develop with watch mode: `npm run dev`
- Lint: `npm run lint:check`
- Type check: `npm run lint:types`

## Publishing

- Packages are published to npm on release via GitHub Actions.
- To publish manually: `npm run publish`
- All packages are public (see `publishConfig` in each package).

## CI/CD

- **ci.yml**: Publishes packages to npm on GitHub release.
- **code-quality.yml**: Runs lint, format, and type checks on push/PR to main and develop.

## Contributing

- Use conventional commits for PRs.
- Add/modify packages in `packages/`.
- Keep code type-safe and well-documented.

---

For more details, see individual package source folders.
