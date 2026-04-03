# PHP AI Harness Template

Harness ready for PHP projects, with guardrails for AI-assisted development.
Follows the same quality-first workflow used in the Go and JavaScript templates.

## Includes

- Base PHP project structure (`src/`, `tests/`).
- **[Mago](https://github.com/carthage-software/mago)** oxidized toolchain (Rust) for:
  - `mago lint` — fast style and semantic linting
  - `mago analyze` — deep semantic analysis
  - `mago format` — PER-CS2.0 code formatting
- **Pest** for unit tests with code coverage reporting.
- **PHPStan** (level 9) for deep type-level static analysis.
- **shipmonk/composer-dependency-analyser** for dead/shadow/misplaced dependency validation.
- **Rector** for automated code refactoring and modernization.
- **Deptrac** for enforcing architectural dependency constraints.
- **Infection** for mutation testing to validate test suite effectiveness.
- **Composer validate** with strict PSR and ambiguity checks.
- **Lefthook** pre-commit and pre-push hooks.
- CI workflow in `.github/workflows/ci.yml` using:
  - `shivammathur/setup-php` for PHP environment setup
  - `shivammathur/cache-extensions` for PHP extension caching
  - `ramsey/composer-install` for dependency installation
- **Composer scripts** for all CLI commands.

## Quick start

1. Copy the template into the target repository.
2. Initialize Git if needed.
3. Install dependencies.
4. Install git hooks.
5. Run verification checks.

Suggested commands:

```sh
cp -R templates/php/. .
composer install
composer doctor
composer verify
```

## Key composer scripts

| Script | What it does |
|---|---|
| `composer verify` | mago lint + test + AI backpressure (fast gate) |
| `composer quality` | verify + mago analyze + phpstan + coverage + mutation + rector check + deptrac + validation |
| `composer format` | format PHP code with mago (PER-CS2.0) |
| `composer format:check` | check formatting without modifying files (`--check`) |
| `composer lint` | run mago lint (fast style + semantic check) |
| `composer lint:fix` | run mago lint with auto-fix (`--fix`) |
| `composer analyze` | run mago semantic analysis |
| `composer phpstan` | run PHPStan deep type analysis (level 9) |
| `composer deps:analyze` | validate dead/shadow/misplaced Composer dependencies |
| `composer deps:analyze:verbose` | dependency validation with verbose usage output |
| `composer test` | run Pest unit tests |
| `composer test:watch` | watch tests and re-run on changes |
| `composer test:coverage` | generate coverage report |
| `composer mutation` | run Infection mutation testing |
| `composer rector` | run Rector auto-refactoring |
| `composer rector:check` | check what Rector would change (dry-run) |
| `composer deptrac` | validate architectural dependencies |
| `composer validate:composer` | validate composer.json configuration |
| `composer validate:autoload` | validate and optimize autoload |
| `composer backpressure` | validate diff size and risk profile |
| `composer doctor` | print local tooling diagnostics |

## Quick reference

Use composer scripts directly:

```sh
composer list
```

## Mago configuration

`mago.toml` configures the three mago tools:

```toml
php-version = "8.3"

[source]
paths = ["src", "tests"]
includes = ["vendor"]

[linter]
# all rules enabled at "error" severity by default

[formatter]
# enforces PER-CS2.0 coding standard

[analyzer]
# deep semantic analysis, excluding tests by default
excludes = ["tests/**"]
```

See the [Mago configuration reference](https://mago.carthage.software/guide/configuration) for full options.

## Coverage threshold

Default minimum coverage is **99%**. Override with:

```sh
COVERAGE_MIN=90 composer test:coverage
```

## Mutation testing threshold

Default minimum MSI (Mutation Score Indicator) is **90%**. Set in `infection.xml`:

```xml
<infection minCoveredMsi="90" minMsi="90">
```

## Architecture layers

`deptrac.yaml` defines allowed dependency directions between layers:

- **Presentation**: HTTP/CLI handlers → App, Domain, Shared
- **App**: Use cases/services → Domain, Shared
- **Domain**: Business logic → Shared
- **Infrastructure**: External integrations → Shared
- **Shared**: Common utilities, no dependencies

Customize layers and rules to match your project's architecture.

## Backpressure

`scripts/ai-backpressure.sh` blocks changes that are too large or risky to keep human review reasonable.

Optional environment variables:

- `MAX_CHANGED_FILES` (default: 25)
- `MAX_TOTAL_ADDED` (default: 900)
- `MAX_LARGEST_FILE_ADDED` (default: 260)

Tune these limits based on the expected PR size in your team:

```sh
MAX_CHANGED_FILES=50 MAX_TOTAL_ADDED=1500 composer backpressure
```

## PHP Version

This template targets **PHP 8.3+**. Adjust `composer.json` and workflow if using different versions:

```json
{
  "require": {
    "php": ">=8.2"
  }
}
```

## CI/CD Workflow

The GitHub Actions workflow in `.github/workflows/ci.yml`:

1. Runs on PHP 8.3 and 8.4
2. Sets up PHP with required extensions
3. Caches dependencies for faster builds
4. Validates composer configuration
5. Runs PHPStan analysis
6. Executes test suite
7. Generates coverage reports
8. Checks code formatting
9. Validates Rector changes
10. Checks architectural dependencies
11. Runs mutation testing
12. Uploads coverage to Codecov (optional)

## Recommended PHP Extensions

For best results, ensure these extensions are installed:

- `json` - JSON support
- `dom` - DOM manipulation
- `mbstring` - Multibyte string functions
- `xdebug` - Code coverage (dev only)

## Tools Configuration

All tools are configured with sensible defaults:

- **mago.toml**: Mago config (lint, analyze, format — PER-CS2.0)
- **phpstan.neon**: Level 9 static analysis with bleeding edge rules
- **rector.php**: PHP 8.3 modernization rules
- **deptrac.yaml**: Layered architecture enforcement
- **pest.xml**: Test suite configuration with coverage
- **infection.xml**: Mutation testing with MSI thresholds
- **composer.json**: Scripts for all CLI commands

## Scripts

Custom scripts in `scripts/`:

- `ai-backpressure.sh`: Validates diff size and risk profile
- `harness-doctor.sh`: Checks tool installation and configuration

Run doctor to diagnose issues:

```sh
composer doctor
```

## Next steps

1. Update `composer.json` with your project name and namespace
2. Customize `deptrac.yaml` for your architecture
3. Add business logic in `src/` following the domain-driven design principles
4. Write tests in `tests/`
5. Configure `.github/workflows/ci.yml` based on your CI/CD needs
6. Review and customize `phpstan.neon` rules for your codebase
7. Adjust coverage and mutation testing thresholds in `pest.xml` and `infection.xml`

## Troubleshooting

**PHP not found**: Ensure PHP 8.3+ is installed and in your PATH
**Composer error**: Run `composer install` to install dependencies
**Tests failing**: Check `tests/bootstrap.php` for proper test configuration
**Script errors**: Run `composer doctor` to diagnose tool issues
**Mutation testing timeout**: Adjust `infection.xml` or increase server timeout

## Composer Scripts Reference

All commands use `composer` - run `composer list` for full list:

**Quick aliases**:
- `composer verify` - Fast gate before commit
- `composer quality` - Full quality suite
- `composer format` - Auto-format all code (mago)
- `composer lint` - Fast mago lint
- `composer analyze` - Mago semantic analysis
- `composer phpstan` - Deep PHPStan type analysis
- `composer deps:analyze` - Composer dependency validation
- `composer test` - Run tests with watch option available
- `composer rector` - Auto-fix code modernization
- `composer deptrac` - Check architecture rules

See `composer.json` for full script definitions.
