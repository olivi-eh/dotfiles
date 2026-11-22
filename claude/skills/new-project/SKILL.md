---
name: new-project
description: Tooling defaults for a new Go, TypeScript, or Python project. Use when creating, scaffolding, bootstrapping, or initializing a new repo, service, library, or CLI, or when adding a new package or module to an existing monorepo. Don't use it to change tooling in an existing project.
---

# Defaults for new projects

Last reviewed: 2026-11-22. If that's more than six months ago, check that the versions and tools below are still current before applying them. If a pick looks outdated or deprecated, say so and propose a replacement. Inside an existing repo, its conventions take precedence.

## All projects
- Use each ecosystem's native tooling for versions and tasks (uv, go, pnpm). Don't add mise or another version manager or task runner
- Use lefthook for fast pre-commit checks; run everything else in CI
- Commit lockfiles; install with frozen lockfiles in CI
- Use Renovate with a minimum release age; also set the package manager's own release-age setting where it has one, so manual installs are covered
- Start with a README, .gitignore, and a project CLAUDE.md (build, test, and lint commands plus conventions)
- Ask which CI platform and licence to use if not obvious

## Python
- Use uv for Python versions, deps, tools, and running commands (`uv run`); commit `uv.lock` and `.python-version`, and set `requires-python`
- Use a `src/` layout with `pyproject.toml`
- Use Ruff for lint and format
- Use ty for type checking, pinned to an exact version (pre-1.0, breaking changes between releases); fall back to pyright strict if it falls short
- Use pytest, plus hypothesis where useful
- Use stdlib `logging` (structlog for structured output), configured only at the entrypoint
- Use pydantic + pydantic-settings for config and validation, httpx for HTTP, typer for CLIs

## Go
- Use the latest stable Go; pin it with the `go` directive in `go.mod` and declare dev tools with `tool` directives
- Use `cmd/<name>/` for binaries and `internal/` for everything else; no `pkg/`
- Use golangci-lint v2, with gofumpt + goimports via `golangci-lint fmt`
- Use stdlib `testing` + `go-cmp`, table-driven tests, and `-race` in CI (no testify)
- Use `log/slog`, passing the logger explicitly rather than as a global
- Use stdlib `net/http` routing; `flag` for CLIs (cobra for multi-command)
- Run `govulncheck` in CI

## TypeScript
- Use TypeScript 7 with `strict`, `noUncheckedIndexedAccess`, `exactOptionalPropertyTypes`, and `verbatimModuleSyntax`; add `isolatedDeclarations` for libraries
- Before adding a tool that uses the TypeScript compiler API, check that it supports TS 7
- ESM only: `"type": "module"` and `"module": "nodenext"`
- Use Node LTS and pnpm: pin pnpm with `packageManager` and Node with `devEngines.runtime`, set pnpm's `minimumReleaseAge`, and define tasks as `package.json` scripts
- Use Biome for lint and format, Vitest for tests
- Use pino for logging, zod for validation at every boundary
- Use tsdown for library builds
