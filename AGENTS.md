# Repository Instructions

## Architecture

This repository is a general-purpose, framework-agnostic JavaScript utility library. Use modern native JavaScript and keep imports safe in browsers and Node.js. Publish both ESM and CommonJS output with minimal runtime dependencies.

## Conventions

- Use JavaScript, primarily in `.js` files. Use `.cjs` only where CommonJS requires it; avoid `.mjs` unless technically necessary.
- Indent JavaScript with tabs and terminate statements with semicolons.
- Prefer native platform features and clear code over dependencies or abstractions.
- Do not introduce framework-specific dependencies, unnecessary polyfills, Babel, or heavy transformation pipelines.

## Compatibility

- Preserve documented public behavior unless a breaking change is intentional and documented.
- Treat `Debug` as a critical compatibility contract, including its constructor, options, methods, prefixes, arguments, activation rules, and return values.
- Keep imports safe when browser globals are unavailable.
- Support native browser modules, Node.js, CommonJS, and standards-compliant frameworks and bundlers.
- Test changes against the built and packed package, including public subpath exports.

## Dependencies and Tests

- Prefer current supported dependencies and major-version upgrades when they improve the project.
- Aim for zero runtime dependencies and remove obsolete tools rather than retaining them to avoid refactoring.
- Add meaningful tests for behavior changes and regression tests for Debug.
- Keep browser-specific tests separate from pure Node.js tests.
- Do not weaken tests to accommodate an upgrade. Run relevant checks before committing.

## Git, Releases, and Agent Work

- Use feature branches and coherent, reviewable commits for significant work.
- Do not publish to npm, create release tags, merge pull requests, or create persistent external services without explicit approval.
- Inspect existing code and tests before changing behavior. Preserve unrelated worktree changes.
- Update documentation when behavior changes and document every intentional breaking change.
- Read `MODERNIZATION_PROGRESS.md` while the 2.0 modernization remains unfinished and update it after significant stages.

