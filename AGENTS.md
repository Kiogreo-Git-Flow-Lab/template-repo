# AGENTS.md - Development Guidelines for AI Agents

This file provides guidelines for AI coding agents operating in this repository.

## Technology Stack

- **Primary Language:** TypeScript
- **Runtime:** Node.js / Bun
- **Package Manager:** pnpm (preferred) / npm / yarn
- **Build Tool:** TypeScript Compiler (tsc)
- **Testing:** Jest or Vitest (when configured)
- **Linting:** ESLint (when configured)

## Build, Lint, and Test Commands

```bash
pnpm type:check    # TypeScript compiler type checking
pnpm lint          # ESLint for code quality
pnpm test          # Run all tests
pnpm test:watch    # Run tests in watch mode
```

### Running a Single Test
```bash
# Jest
pnpm test -- -t "test name"
# Vitest
pnpm vitest run src/module.test.ts
```

### Full Pipeline
```bash
pnpm type:check && pnpm lint && pnpm test
```

## Code Style Guidelines

### Core Philosophy
**Modular, Functional, Maintainable** - Write small, focused, reusable components with pure functions.

### Naming Conventions
- **Files:** `lowercase-with-dashes.ts` (e.g., `user-service.ts`)
- **Functions:** `verbPhrase` (e.g., `getUser`, `validateEmail`)
- **Predicates:** `is`/`has`/`can` prefix (e.g., `isValid`, `hasPermission`)
- **Variables:** Descriptive, `const` by default (e.g., `userCount` not `uc`)
- **Constants:** `UPPER_SNAKE_CASE` (e.g., `MAX_RETRY_COUNT`)
- **Types:** PascalCase (e.g., `UserProfile`, `ApiResponse`)

### Imports and Module Organization
```typescript
import { useState, useEffect } from 'react';
import { z } from 'zod';

import { UserService } from './services/user.service';
import { ApiClient } from './utils/api-client';
import type { User, CreateUserDto } from '../types';

component/
├── index.ts      # Public interface
├── core.ts       # Core logic (pure functions)
├── utils.ts      # Helpers
└── tests/        # Tests
```

### Formatting
- 2-space indentation
- Max 100 lines/file (ideally < 50)
- Max 50 lines/function
- Max 3 nesting levels (use early returns)
- Semicolons required
- Single quotes for strings

### Type Safety
```typescript
interface User {
  id: string;
  name: string;
  email: Email;
  role: 'admin' | 'user' | 'guest';
}
// ❌ Avoid any
```

### Error Handling
```typescript
// ✅ Result pattern
function parseJSON(text: string): Result<unknown, Error> {
  try {
    return { success: true, data: JSON.parse(text) };
  } catch (error) {
    return { success: false, error: error as Error };
  }
}

// ✅ Validate at boundaries
function createUser(userData: unknown): Result<User, ValidationError> {
  const validation = validateUserData(userData);
  if (!validation.isValid) {
    return { success: false, errors: validation.errors };
  }
  return { success: true, user: saveUser(validation.data) };
}
```

### Functional Programming & Dependencies
```typescript
// ✅ Pure functions
const add = (a: number, b: number): number => a + b;
const formatUser = (user: User): FormattedUser => ({
  ...user,
  fullName: `${user.firstName} ${user.lastName}`
});

// ✅ Immutability
const addItem = <T>(items: readonly T[], item: T): T[] => [...items, item];

// ✅ Declarative
const activeUsers = users.filter(u => u.isActive).map(u => u.name);

// ✅ Explicit dependencies via injection
function createUserService(database: Database, logger: Logger): UserService {
  return {
    createUser: (data) => {
      logger.info('Creating user');
      return database.insert('users', data);
    }
  };
}
// ❌ Hidden dependencies, console.log in pure functions
```

### Security Guidelines
- **NEVER** hardcode credentials or secrets
- Use environment variables for all sensitive data
- Sanitize all user input
- Use parameterized queries (prevent SQL injection)
- **NEVER** log passwords, tokens, or API keys

## Anti-Patterns to Avoid

- ❌ **Mutation:** `items.push(item)` → `[...items, item]`
- ❌ **Side effects:** console.log in pure functions → Logger injection
- ❌ **Deep nesting:** 5+ levels → Early returns
- ❌ **God modules:** 200+ lines → Split by concern
- ❌ **Global state:** `global.config` → Dependency injection
- ❌ **Large functions:** 100+ lines → < 50 lines
- ❌ **Magic values:** `if (status === 1)` → `const ACTIVE = 1`

## Development Workflow

1. Plan → Approve → Load Context → Implement Incrementally → Validate → Handoff

## Quality Gates

- [ ] TypeScript compilation passes
- [ ] Linting passes
- [ ] Tests pass (> 80% coverage)
- [ ] Code is modular and maintainable
- [ ] No security vulnerabilities
- [ ] Documentation updated
