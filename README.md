# Cursor Rules Index

Standards and policies for monorepos following hexagonal architecture, AWS serverless, and TypeScript.

## Suggested Reading Order

For new team members or when onboarding to a project:

1. **architecture.mdc** — Hexagonal architecture, monorepo layout
2. **platform.mdc** — AWS, Lambda, Middy, local dev
3. **domain-entity-classes.mdc** — Entity structure
4. **use-cases.mdc** — Use case orchestration and handler wiring
5. **api-design.mdc** — REST API standards
6. **error-handling.mdc** — Error format and mapping
7. **authorisation.mdc** — Janus + Cedar
8. **security.mdc** — OWASP, multi-tenancy
9. **testing.mdc** — Honeycomb, Definition of Done

## How Rules Are Applied

- **alwaysApply: true** — The rule is always included in context when working in the project.
- **globs** — The rule is included when the current file matches one of the glob patterns. Use for file-type or path-specific guidance (e.g. handlers, domain, frontend).
- Rules with neither `alwaysApply` nor `globs` are not automatically applied; they serve as reference.

## Rules Overview

### Architecture & Platform

| Rule | Description | When Applied |
|------|-------------|--------------|
| [architecture.mdc](./architecture.mdc) | Hexagonal architecture, ports/adapters, REST vs GraphQL BFF, monorepo layout | Always |
| [platform.mdc](./platform.mdc) | AWS, Lambda, PowerTools, Middy, DynamoDB, Cognito, local dev | Always |
| [infra-stack-policy.mdc](./infra-stack-policy.mdc) | Auth, Stateful, Stateless CDK stacks | `**/infra/**/*.ts`, `**/cdk/**/*.ts` |
| [adr.mdc](./adr.mdc) | Architecture Decision Record format | `**/adr/**/*.md` |

### Domain & Data

| Rule | Description | When Applied |
|------|-------------|--------------|
| [domain-entity-classes.mdc](./domain-entity-classes.mdc) | Domain entities as classes with factories and validation | `**/domain/**/*.ts` |
| [use-cases.mdc](./use-cases.mdc) | Use case structure, orchestration, handler wiring, ports-only | `**/core/**/*.ts`, `**/use-cases/**/*.ts`, `**/handlers/**/*.ts`, `**/lambda/**/*.ts` |
| [data-access.mdc](./data-access.mdc) | Repository patterns, DynamoDB access, soft delete, TTL | `**/repositories/**/*.ts`, `**/adapters/**/*.ts`, `**/domain/**/ports/**/*.ts`, `**/domain/**/interfaces/**/*.ts` |

### API & Handlers

| Rule | Description | When Applied |
|------|-------------|--------------|
| [api-design.mdc](./api-design.mdc) | REST API design — pagination, versioning, idempotency, CORS, OpenAPI, health checks | Handlers, OpenAPI files |
| [error-handling.mdc](./error-handling.mdc) | API error format, typed errors, HTTP status mapping | Handlers |
| [observability.mdc](./observability.mdc) | Structured logging, correlation IDs, log hygiene | Handlers |
| [events.mdc](./events.mdc) | Event schema, idempotency, DLQ, retries | Event handlers |

### Frontend

| Rule | Description | When Applied |
|------|-------------|--------------|
| [frontend.mdc](./frontend.mdc) | Component structure, state, a11y, testing | `**/apps/web/**`, `**/apps/frontend/**`, `**/packages/ui/**`, `**/packages/components/**` |

### Quality & DevOps

| Rule | Description | When Applied |
|------|-------------|--------------|
| [authorisation.mdc](./authorisation.mdc) | Janus + Cedar authorisation pattern | Always |
| [security.mdc](./security.mdc) | OWASP, defense in depth, multi-tenancy | Always |
| [git-commits.mdc](./git-commits.mdc) | Conventional Commits, GitHub Flow | Always |
| [quality-checks-after-task.mdc](./quality-checks-after-task.mdc) | Run checks after each task; Definition of Done | Always |
| [testing.mdc](./testing.mdc) | Honeycomb methodology, Vitest, Playwright | `**/*.test.ts`, `**/*.spec.ts` |
| [typescript-standards.mdc](./typescript-standards.mdc) | Strict mode, naming, typed errors, TSDoc | `**/*.ts` |
| [ci-cd.mdc](./ci-cd.mdc) | CI/CD pipelines, branch protection, deployment | `**/.github/**/*.yml` |

## Cross-References

| From | To | Relationship |
|------|-----|--------------|
| Architecture | infra-stack-policy, platform, api-design, use-cases | Stack layout, infra, API standards, use case structure |
| Platform | authorisation, observability | Auth pattern, logging |
| Error handling | typescript-standards | Typed error pattern |
| API design | error-handling | Error format |
| Quality checks | testing | Definition of Done |
| Data access | error-handling, api-design | Error mapping, pagination |
| Events | observability | Correlation IDs in payloads |
| Frontend | testing | Component and E2E tests |
| Use cases | data-access, events | Repositories, event publishing |
| Handlers (error-handling, api-design, observability) | use-cases | Handler wiring, thin adapters |

## Handler Path Conventions

Rules that target handlers use these common paths. Align your project structure for best rule matching:

- `**/handlers/**/*.ts`
- `**/lambda/**/*.ts`
- `**/src/handlers/**/*.ts`
