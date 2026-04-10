# @brandflow/types

Shared TypeScript types and interfaces for BrandFlow Control.

## Purpose

This package defines common domain types used across frontend, backend, and worker packages.

## Usage

Import shared types from `@brandflow/types`:

```ts
import { PlatformKey, WorkflowStatus } from "@brandflow/types";
```
# @brandflow/types

Shared TypeScript types and interfaces for BrandFlow Control.

## Purpose

This package defines common domain types used across frontend, backend, and worker packages.

## Usage

Import shared types from `@brandflow/types`:

```ts
import { PlatformKey, WorkflowStatus } from "@brandflow/types";
```
# @brandflow/api

NestJS backend API for BrandFlow Control.

## Purpose

This package implements the REST API, domain modules, authentication scaffolding, and worker orchestration entrypoints for the backend.

## Key folders

- `src/` — API source code
- `src/modules/` — backend feature modules
- `src/common/` — shared guards, config, DTOs

## Scripts

- `pnpm dev` — run NestJS in watch mode
- `pnpm build` — compile TypeScript
- `pnpm lint` / `pnpm typecheck` — run `tsc` against `tsconfig.json`

## Notes

The API currently uses workspace packages for shared config and types (`@brandflow/config`, `@brandflow/types`).

## Notes

Keeping shared types in one package helps ensure consistency across the monorepo.

## Notes

Keeping shared types in one package helps ensure consistency across the monorepo.
