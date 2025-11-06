# AGENTS.md

## Build & Testing
- No build step required - GitHub Actions run JS files directly
- No test suite configured - test locally via action invocation
- Lint/format: No automated linting configured

## Code Style
- **Language**: JavaScript (Node.js, CommonJS modules)
- **Imports**: Use `const { x } = require('module')` at top of file
- **Functions**: Async/await over promises; destructure params as objects `async function fn({ param1, param2 })`
- **Naming**: camelCase for functions/variables; UPPER_SNAKE for constants; descriptive names
- **Comments**: JSDoc block comments for exported functions with `@param` tags
- **Logging**: Configurable via LOG_LEVEL env var (error, warn, info, debug); use console.log/warn/error
- **Error handling**: Try-catch with graceful degradation; log errors but don't fail unnecessarily
- **Strings**: Template literals for interpolation; single line when possible

## Conventions
- PR titles: Conventional commits (`type(scope): description`)
- Label-driven workflow: Respect existing label definitions in LABEL_DEFINITIONS
- Defensive coding: Check for missing tokens/credentials and skip gracefully
- Delta-based updates: Calculate changes before API calls to minimize operations
- No package.json: Actions use GitHub's node runtime directly
