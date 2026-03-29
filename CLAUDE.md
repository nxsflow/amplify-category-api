# Amplify Category API (nxsflow fork)

Fork of `aws-amplify/amplify-category-api`. See skill `branching-strategy` for the fork workflow.

See `AGENTS.md` for commands, testing workflow, code quality rules, and E2E process.

## What This File Adds

This file covers project knowledge not in AGENTS.md: architecture, tooling specifics, release process, and fork context.

## Monorepo Tooling

- **Package manager**: Yarn Classic (v1) with workspaces
- **Orchestration**: Lerna (independent versioning, conventional commits)
- **Build**: TypeScript composite projects — target ES2018, module CommonJS, strict mode
- **Test framework**: Jest with `ts-jest` — 90% coverage threshold (branches, functions, lines)
- **Formatter**: Prettier — single quotes, trailing commas, 140 print width, semicolons
- **No git hooks** — all validation happens in CI

## Architecture

- **GraphQL Transformers**: Each `amplify-graphql-*-transformer` package handles one directive (`@model`, `@auth`, `@function`, `@hasOne`, etc.)
- **Transformer Core**: `amplify-graphql-transformer-core` orchestrates the transform pipeline
- **API Construct**: `amplify-graphql-api-construct` is the CDK L3 construct consumers use
- **Data Construct**: `amplify-data-construct` wraps the API construct for Amplify Gen 2
- **Interfaces**: `amplify-graphql-transformer-interfaces` defines shared types

### Key packages for Bedrock/AI work

- `amplify-graphql-generation-transformer` — handles `@generation` directive (Bedrock inference)
- `amplify-graphql-conversation-transformer` — handles `@conversation` directive (Bedrock chat)

## Release Process

This repo uses **Lerna conventional commits** for versioning (NOT changesets):

- `yarn publish:release` — graduate and publish production releases
- `yarn publish:beta` — publish beta pre-releases
- `yarn publish:main` — publish canary/alpha releases
- Commit messages drive version bumps: `feat:` = minor, `fix:` = patch, `BREAKING CHANGE:` = major

## PR Process

- PR template at `.github/PULL_REQUEST_TEMPLATE.md` (includes CDK/CFN params section)
- CODEOWNERS: `@aws-amplify/amplify-data` for general, `@aws-amplify/amplify-data-admins` for API.md/CI/deps
- Commit style: conventional commits (`feat:`, `fix:`, `chore:`) with Lerna package scopes
