# Coding Standards

> Provenance: derived from [ScottLabs-DevStudio/engineering-standards@18b06c7](https://github.com/ScottLabs-DevStudio/engineering-standards/tree/18b06c7148277cb74a6c9db3dcdcff5c42eb74c9) (base + add-ons: python), which adapts [microsoft/hve-core@6cd810c](https://github.com/microsoft/hve-core/tree/6cd810c7b9448d98ea07db0a7a9124824d32dacc) (`6cd810c7b9448d98ea07db0a7a9124824d32dacc`, MIT, text only). Repo-specific rules added below take precedence.

Vendored or upstream code (copied from another project) is exempt from these standards; apply them to our own changes only.

## Correctness

- **Untrusted input:** Treat decoded payloads, request bodies, files, env values, and tool or model output as untrusted. Validate shape, types, finite numbers, and dates before use or persistence.
- **Boundaries:** Keep server-owned configuration and secrets on the server. Pass resolved settings to consumers through explicit, persisted, or stamped state, not by re-deriving them.
- **Error handling:** On a failed read, write, parse, or call, keep retryable input and the last good state. Do not advance cursors, acknowledge messages, or report success as if the input was consumed. Let tools fail with their native error messages; do not swallow stderr.
- **Time:** Inject one clock instant and use it consistently across decisions, rollover, and display. Cover clock-sensitive logic with pinned-time tests, including backward clock moves.
- **Identity:** Key rows, caches, and automation selectors on stable IDs, never on display text.

## Tests

- For changed behavior, cover each distinct branch with explicit negative and control cases.
- A regression test must fail when the guard it protects is reverted.
- Pin time, randomness, and network in tests; no live external calls in unit tests.

## Documentation

- For new or materially changed reusable APIs, write a short caller-facing doc comment covering only contracts that names and types don't make clear: units and ranges, side effects, ordering, empty or missing results, failure and async behavior. Keep it accurate as behavior changes. Don't retrofit unchanged code or comment generated code, mocks, or self-describing constants.
- Comments explain why (non-obvious conditionals, regexes, workarounds), not what.

## Git and review

- Conventional Commit subjects in imperative mood (`feat`, `fix`, `refactor`, `perf`, `test`, `docs`, `build`, `ops`, `chore`), under 100 bytes; optional short bulleted body.
- One push per reviewable unit. Merge commits only: never rebase or force-push shared branches.
- Answer every review thread before ship.
- No secrets in repos, argv, URLs, CI output, logs, or client bundles. No new keys or paid services without approval.
- Pin dependencies (lockfiles) and pin third-party GitHub Actions to full commit SHAs. Workflows default to `permissions: contents: read`; grant more per job only when needed, never `write-all`.
- Security review uses [security/owasp-checklist.md](https://github.com/ScottLabs-DevStudio/engineering-standards/blob/18b06c7148277cb74a6c9db3dcdcff5c42eb74c9/security/owasp-checklist.md).

## Provenance

Commit, documentation, and workflow rules adapted from [microsoft/hve-core](https://github.com/microsoft/hve-core) at `6cd810c7b9448d98ea07db0a7a9124824d32dacc`: `.github/instructions/hve-core/commit-message.instructions.md`, `.github/instructions/coding-standards/typescript/typescript-doc-comments.instructions.md` (generalized), `.github/instructions/workflows.instructions.md` (pinning and permissions), `.github/instructions/coding-standards/bash/bash.instructions.md` (error visibility). Correctness and test rules generalized from ScottLabs-DevStudio/DEH-Forex PR #47. Standards text only; see [NOTICE](https://github.com/ScottLabs-DevStudio/engineering-standards/blob/18b06c7148277cb74a6c9db3dcdcff5c42eb74c9/NOTICE).

## Add-on: Python

- **Environment:** Use the repo's declared tool (`uv` preferred: `uv add`, `uv sync`, `uv run`); don't `pip install` into a global interpreter. Commit the lockfile (`uv.lock` or pinned requirements).
- **Entry points:** `main() -> int` with `if __name__ == "__main__": sys.exit(main())`. Exit codes: 0 success, 1 failure, 2 argument or config error, 130 interrupt. Handle `KeyboardInterrupt` and `BrokenPipeError` at top level.
- **CLI:** Build the parser in its own function (`create_parser()`) for testability; `type=Path` for files, `store_true` for flags. Use click for complex subcommand CLIs.
- **Logging:** Module-level `logger = logging.getLogger(__name__)`, configured once in `main`. Never log secrets.
- **Paths:** `pathlib.Path`, not `os.path`.
- **Subprocess:** `subprocess.run([...], check=True, capture_output=True, text=True)` with a list argv; no `shell=True` with interpolated input; secrets via env, not argv.
- **Types:** 3.11+ syntax: `list[str]`, `str | None`, `Literal`, `Self`.
- **Docs:** Google-style docstrings (Args, Returns, Raises) for public functions; module docstring with usage for scripts.
- **Layout:** imports (stdlib, third-party, local), constants, logger, helpers, parser, run logic, `main`, module guard.
- **Tests (pytest):** files `test_<module>.py`; names `test_given_<context>_when_<action>_then_<expected>` (or the repo's existing convention); shared fixtures in `conftest.py`; prefer pytest-mock `mocker.patch` for spies, `monkeypatch` for simple env or attribute overrides.

Sources at hve-core `6cd810c7b9448d98ea07db0a7a9124824d32dacc`: `.github/instructions/coding-standards/python-script.instructions.md`, `.github/instructions/coding-standards/python-tests.instructions.md`, `.github/instructions/coding-standards/uv-projects.instructions.md`. Microsoft copyright headers from upstream are not adopted.
