# JavaScript AI Harness Template

Harness ready for JavaScript projects, with guardrails for AI-assisted development.
Structured to cover both Node and browser runtimes with the same quality-first workflow used in the Go template.

## Includes

- Base project structure for `src/node`, `src/browser`, and shared modules.
- `vitest` for unit tests and browser-environment tests with `happy-dom`.
- `eslint` (with dual-runtime globals) and `prettier` (default config) for linting and formatting.
- `lefthook` pre-commit and pre-push hooks.
- Coverage gate with a configurable minimum threshold.
- `npm audit` security checks.
- CI workflow in `.github/workflows/quality.yml` with separate jobs per quality concern.

## Quick start

1. Copy the template into the target repository.
2. Install dependencies.
3. Install git hooks.
4. Run the verification checks.

Suggested commands:

```sh
cp -R templates/javascript/. .
npm install
npx lefthook install
npm run verify
```

## Key scripts

| Script | What it does |
|---|---|
| `npm run verify` | lint + unit tests + AI backpressure (fast gate) |
| `npm run quality` | verify + coverage gate + security + artifacts |
| `npm run format` | format supported text files with Prettier |
| `npm run lint` | run ESLint |
| `npm run test` | run unit and browser-environment tests |
| `npm run coverage` | generate coverage report and enforce minimum threshold |
| `npm run security` | run `npm audit` with `high` severity threshold |
| `npm run dev:node` | execute `src/node/index.js` |
| `npm run dev:browser` | serve `index.html` locally |

## Hook pipeline

```text
pre-commit (parallel, skips merge/rebase):
	setup:    prettier on staged text files (auto-fixed)
	jobs:     lint · test · coverage · security
```

## Coverage threshold

Default minimum coverage is **99%**. Override with:

```sh
COVERAGE_MIN=90 npm run coverage
```

## Backpressure

`scripts/ai-backpressure.mjs` blocks changes that are too large or risky to keep human review reasonable.

Optional variables:

- `MAX_CHANGED_FILES` (default: 25)
- `MAX_TOTAL_ADDED` (default: 900)
- `MAX_LARGEST_FILE_ADDED` (default: 260)

Tune these limits based on the expected PR size in your team.
