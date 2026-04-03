# Go AI Harness Template

Harness ready for Go projects, with guardrails for AI-assisted development.
Based on the backpressure and validation approach used in [`iyaki/ralph`](https://github.com/iyaki/ralph).

## Includes

- Base Go project structure (`cmd`, `internal`, `test`).
- Makefile with `verify` and `quality` pipelines.
- Lefthook pre-commit and pre-push hooks.
- `golangci-lint` — fast linting across the codebase.
- `go-arch-lint` — enforce architectural dependency rules.
- `govulncheck` + `gosec` — security vulnerability and code scanning.
- `gremlins` — mutation testing to validate test suite effectiveness.
- `scripts/ai-backpressure.sh` — blocks oversized or risky AI-generated diffs.
- `scripts/harness-doctor.sh` — checks that all required tools are installed.
- CI workflow (`.github/workflows/ci.yml`) running `make verify`.

## Quick start

1. Copy the template contents into the target repository.
2. Update the module name in `go.mod` to match your project.
3. Install required CLI tooling.
4. Run verification checks.

Suggested commands:

```sh
cp -R templates/go/. .
go mod init your.org/your-project
make tools
make verify
```

## Key make targets

| Target | What it does |
|---|---|
| `make verify` | lint + test + AI backpressure (fast gate) |
| `make quality` | verify + race detection + coverage gate + security + arch |
| `make format` | gofmt on all tracked Go files |
| `make lint` | golangci-lint |
| `make test` | unit tests |
| `make test-race` | unit tests with race detector |
| `make test-coverage` | enforce ≥85% coverage threshold |
| `make mutation` | gremlins mutation testing |
| `make security` | govulncheck + gosec |
| `make arch` | go-arch-lint architectural check |
| `make backpressure` | validate diff size and risk profile |
| `make doctor` | print local tooling diagnostics |
| `make tools` | install all required CLI tools |
| `make run` | run `cmd/app` from source |

## Hook pipeline

```
pre-commit (parallel, skips merge/rebase):
  setup:    gofmt (staged files auto-fixed)
  jobs:     test · lint · test-coverage · mutation · security · arch

pre-push:
  jobs:     verify
```

## Architectural constraints

`.go-arch-lint.yml` defines allowed dependency directions between packages.
Edit it to match your project's layer boundaries (`cmd`, `domain`, `service`, `transport`, `platform`).

## Coverage threshold

Default minimum coverage is **99%**. Override with:

```sh
make test-coverage COVERAGE_MIN=90
```
