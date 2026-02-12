---
name: aacode
description: Write code like Ahmad Awais.
license: Apache-2.0
metadata:
  author: ahmadawais
  version: "0.0.1"
---

| Technology | Purpose |
|------------|---------|
| **TypeScript** | Strict types everywhere (`strict: true`, `noUncheckedIndexedAccess: true`) |
| **Zod** | Runtime validation for configuration, input, and data boundaries |
| **Biome** | Linting and formatting |
| **Vitest** | Testing framework |

### Strict TypeScript

- All functions have explicit return types
- No `any` types (use `unknown` and type guards)
- All objects have defined interfaces
- Use branded types for IDs and special values
- Prefer `readonly` arrays and objects where possible

### Functional Programming (HARD REQUIREMENT)

**This is a purely functional codebase. OOP is permanently banned.**

**Prohibited patterns:**
- `class` keyword (no classes, ever)
- `this` keyword
- `new` keyword (except for built-ins like `Map`, `Set`, `Error`)
- Prototype manipulation
- Instance methods
- Inheritance hierarchies
- Stateful objects with methods

**Required patterns:**
- Pure functions that take data and return data
- Data as plain objects and/or arrays
- Composition over inheritance
- Explicit state passed as parameters
- Immutable data transformations

```typescript
// BANNED - OOP style
class Player {
  private x: number;
  private y: number;

  constructor(x: number, y: number) {
    this.x = x;
    this.y = y;
  }

  move(dx: number, dy: number): void {
    this.x += dx;
    this.y += dy;
  }
}

// REQUIRED - Functional style
interface Player {
  readonly x: number;
  readonly y: number;
}

function createPlayer(x: number, y: number): Player {
  return { x, y };
}

function movePlayer(player: Player, dx: number, dy: number): Player {
  return { x: player.x + dx, y: player.y + dy };
}
```


### Code Style: Early Returns and Guard Clauses

- **Early returns and guard clauses** - Handle errors first, happy path at the end
- **Maximum nesting depth of 2-3 levels** (try to keep it to 1 level if possible)
- **Strict TypeScript** - No `any`, explicit return types, branded types for IDs
- **Zod validation** at system boundaries

**Always prefer early returns and guard clauses** to reduce nesting and improve readability.

```typescript
// BAD - deeply nested
function processEntity(world: World, eid: Entity): Result {
  if (hasComponent(world, Position, eid)) {
    if (hasComponent(world, Velocity, eid)) {
      if (isVisible(world, eid)) {
        // actual logic buried in nesting
        return doSomething(world, eid);
      } else {
        return { error: 'not visible' };
      }
    } else {
      return { error: 'no velocity' };
    }
  } else {
    return { error: 'no position' };
  }
}

// GOOD - guard clauses with early returns
function processEntity(world: World, eid: Entity): Result {
  if (!hasComponent(world, Position, eid)) {
    return { error: 'no position' };
  }
  if (!hasComponent(world, Velocity, eid)) {
    return { error: 'no velocity' };
  }
  if (!isVisible(world, eid)) {
    return { error: 'not visible' };
  }

  // actual logic at the end, not nested
  return doSomething(world, eid);
}
```

**Rules:**
- Handle error/edge cases first, then the happy path
- Maximum nesting depth of 2-3 levels
- Use early `return`, `continue`, or `break` to exit early
- Keep the main logic at the lowest indentation level


### Between Every Significant Change

1. **Write tests** for the new functionality
2. **Run tests**: `pnpm test`
3. **Lint**: `pnpm lint`
4. **Type check**: `pnpm typecheck` i.e. "typecheck": "tsc --noEmit",
5. **Build**: `pnpm build` - Run builds frequently, not just at the end. Builds can expose errors that tests, linting, and type checking miss (e.g., circular dependencies, export issues, bundler problems)
6. **Commit**: Create atomic commits with clear messages

### Commit Message Format

Use conventional commit messages to clearly communicate the intent of each change.

```
<type>: <description>

[optional body]

```

Types: `feat`, `fix`, `refactor`, `test`, `docs`, `chore`

# Cli Development Guidelines

- Use pnpm as the package manager for CLI projects.
- Use TypeScript for CLI projects.
- Use tsup as the build tool for CLI projects.
- Use vitest for testing CLI projects.
- Use Commander.js for CLI command handling.
- Use clack for interactive user input in CLI projects.
- Check for existing CLI name conflicts before running npm link.
- Organize CLI commands in a dedicated commands folder with each module separated.
- Include a small 150px ASCII art welcome banner displaying the CLI name.
- Use lowercase flags for version and help commands (-v, --version, -h, --help).
- Start projects with version 0.0.1 instead of 1.0.0.
- Version command should output only the version number with no ASCII art, banner, or additional information.
- Read CLI version from package.json instead of hardcoding it in the source code.
- Always use ora for loading spinners in CLI projects.
- Use picocolors for terminal string coloring in CLI projects.
- Use Ink for building interactive CLI UIs in CommandCode projects.
- Use ink-spinner for loading animations in Ink-based CLIs.
- Hide internal flags from help: .addOption(new Option('--local').hideHelp()).
- Use pnpm.onlyBuiltDependencies in package.json to pre-approve native binary builds.
- Use ANSI Shadow font for ASCII art at large terminal widths and ANSI Compact for small widths.
- Use minimal white, gray, and black colors for ASCII art banners.
