# Cursor Rules: JavaScript Strict Mode Configuration & Analysis

## Overview

This document provides comprehensive guidance on JavaScript strict mode implementation, configuration best practices, and analysis tools for enforcing strict mode compliance across projects.

---

## 1. Strict Mode Fundamentals

### What is Strict Mode?

Strict mode is an opt-in mechanism that enforces stricter parsing and error handling rules in JavaScript code. It helps catch common coding mistakes and prevents unsafe actions.

### Enabling Strict Mode

#### Global Scope
```javascript
'use strict';

// All code below runs in strict mode
var x = 3.14;
```

#### Function Scope
```javascript
function myFunction() {
  'use strict';
  // Only this function runs in strict mode
  var x = 3.14;
}
```

#### Module Scope (ES6)
```javascript
// In ES6 modules, strict mode is automatic
export const myFunction = () => {
  // Automatically in strict mode
};
```

#### Class Scope
```javascript
class MyClass {
  // Classes are automatically in strict mode
  constructor() {
    this.value = 10;
  }
}
```

---

## 2. Strict Mode Rules & Restrictions

### 2.1 Variable Declaration Requirements

**Rule:** Variables must be declared before use.

```javascript
'use strict';

// ❌ NOT ALLOWED - Creates global variable
function badExample() {
  x = 3.14;  // Error: x is not defined
}

// ✅ CORRECT - Properly declared
function goodExample() {
  var x = 3.14;
  let y = 2.71;
  const z = 1.41;
}
```

### 2.2 Object Property Restrictions

**Rule:** Cannot delete object properties using delete operator without proper context.

```javascript
'use strict';

const obj = { x: 10 };

// ❌ NOT ALLOWED
delete obj.x;  // Error: Cannot delete property

// ✅ CORRECT - Using Object.defineProperty with configurable
const obj2 = {};
Object.defineProperty(obj2, 'x', {
  value: 10,
  configurable: true
});
delete obj2.x;  // OK
```

### 2.3 Property Name Restrictions

**Rule:** Duplicate property names in object literals are not allowed.

```javascript
'use strict';

// ❌ NOT ALLOWED
const obj = {
  name: 'John',
  name: 'Jane'  // Error: Duplicate property
};

// ✅ CORRECT
const obj = {
  firstName: 'John',
  lastName: 'Jane'
};
```

### 2.4 Function Parameter Restrictions

**Rule:** Duplicate parameter names are not allowed.

```javascript
'use strict';

// ❌ NOT ALLOWED
function add(a, a, c) {  // Error: Duplicate parameter
  return a + a + c;
}

// ✅ CORRECT
function add(a, b, c) {
  return a + b + c;
}
```

### 2.5 Function Names Reserved

**Rule:** Cannot use certain reserved words as function names.

```javascript
'use strict';

// ❌ NOT ALLOWED
const eval = 5;        // Error: eval is reserved
const arguments = 10;  // Error: arguments is reserved

// ✅ CORRECT
const evaluate = 5;
const params = 10;
```

### 2.6 The `eval()` Function

**Rule:** eval() creates variables in local scope, not global.

```javascript
'use strict';

eval('var x = 10');
console.log(x);  // ReferenceError: x is not defined

// eval scope is isolated
const x = 5;
eval('var x = 10');
console.log(x);  // 5 (unchanged)
```

### 2.7 The `with` Statement

**Rule:** The `with` statement is not allowed.

```javascript
'use strict';

const obj = { name: 'John' };

// ❌ NOT ALLOWED
with (obj) {  // SyntaxError: Unexpected token 'with'
  console.log(name);
}

// ✅ CORRECT
console.log(obj.name);
```

### 2.8 Function Context (`this`)

**Rule:** `this` is `undefined` in function calls (not the global object).

```javascript
'use strict';

function myFunction() {
  console.log(this);  // undefined (not window/global)
}

myFunction();

// Exception: this is preserved with method calls
const obj = {
  name: 'John',
  greet() {
    console.log(this.name);  // 'John'
  }
};

obj.greet();
```

### 2.9 Octal Syntax

**Rule:** Octal number syntax is not allowed.

```javascript
'use strict';

// ❌ NOT ALLOWED
const num = 010;  // SyntaxError: Octal literals are not allowed

// ✅ CORRECT
const num = 8;
const octal = 0o10;  // Binary: 8
```

### 2.10 Escape Sequences

**Rule:** Certain escape sequences are restricted.

```javascript
'use strict';

// ❌ NOT ALLOWED
const str = '\010';  // SyntaxError

// ✅ CORRECT
const str = '\u0008';
```

---

## 3. Strict Mode Configuration Tools

### 3.1 ESLint Configuration

**File: `.eslintrc.json`**

```json
{
  "env": {
    "es2021": true,
    "node": true,
    "browser": true
  },
  "parserOptions": {
    "ecmaVersion": "latest",
    "sourceType": "module"
  },
  "rules": {
    "strict": ["error", "global"]
  },
  "overrides": [
    {
      "files": ["*.js"],
      "rules": {
        "strict": ["error", "global"]
      }
    },
    {
      "files": ["*.mjs"],
      "rules": {
        "strict": "off"
      }
    }
  ]
}
```

### 3.2 Package.json Configuration

```json
{
  "name": "strict-mode-project",
  "version": "1.0.0",
  "type": "module",
  "scripts": {
    "lint": "eslint .",
    "lint:strict": "eslint . --rule 'strict:error'",
    "test": "jest",
    "check": "npm run lint && npm test"
  },
  "devDependencies": {
    "eslint": "^8.0.0",
    "@eslint/js": "^8.0.0",
    "jest": "^29.0.0"
  }
}
```

### 3.3 Babel Configuration

**File: `.babelrc.json`**

```json
{
  "presets": [
    [
      "@babel/preset-env",
      {
        "targets": {
          "browsers": ["> 0.5%, last 2 versions, not dead"],
          "node": "current"
        }
      }
    ]
  ],
  "plugins": [
    "@babel/plugin-syntax-strict-mode"
  ]
}
```

### 3.4 TypeScript Configuration

**File: `tsconfig.json`**

```json
{
  "compilerOptions": {
    "target": "ES2020",
    "module": "ESNext",
    "strict": true,
    "alwaysStrict": true,
    "checkJs": true,
    "allowJs": true,
    "moduleResolution": "node"
  },
  "include": ["src/**/*"],
  "exclude": ["node_modules", "dist"]
}
```

---

## 4. Strict Mode Best Practices

### 4.1 Default to Strict Mode

**Recommendation:** Always enable strict mode globally for new projects.

```javascript
'use strict';

// Project-wide strict mode enforcement
// Applied to all files at the top level
```

### 4.2 Module Systems

**Modern Approach:** Use ES6 modules (automatic strict mode).

```javascript
// app.js - Automatically in strict mode
export function calculate(a, b) {
  return a + b;
}

// main.js - Automatically in strict mode
import { calculate } from './app.js';

console.log(calculate(5, 3));
```

### 4.3 Avoid Global Strict Mode for Legacy Code

**Pattern:** Use function-scoped strict mode when migrating.

```javascript
// Can mix strict and non-strict code
function strictFunction() {
  'use strict';
  // Strict mode here
}

function normalFunction() {
  // Non-strict mode here
}
```

### 4.4 Library Best Practices

**For Libraries:** Document strict mode requirements.

```javascript
/**
 * MyLibrary - Requires Strict Mode
 * @requires 'use strict';
 * 
 * All consumers of this library should enable strict mode:
 * 'use strict';
 * import MyLibrary from './my-library';
 */

'use strict';

export class MyLibrary {
  constructor() {
    this.version = '1.0.0';
  }
}
```

---

## 5. Common Strict Mode Errors & Solutions

### Error 1: Global Variable Assignment

**Problem:**
```javascript
'use strict';

function setup() {
  config = { debug: true };  // ReferenceError
}
```

**Solution:**
```javascript
'use strict';

let config;

function setup() {
  config = { debug: true };  // OK
}
```

### Error 2: Deleting Plain Variables

**Problem:**
```javascript
'use strict';

let x = 10;
delete x;  // TypeError
```

**Solution:**
```javascript
'use strict';

const obj = { x: 10 };
delete obj.x;  // OK for configurable properties

// Or use Object.freeze
Object.freeze(obj);
```

### Error 3: Modifying Read-Only Properties

**Problem:**
```javascript
'use strict';

const obj = Object.freeze({ x: 10 });
obj.x = 20;  // TypeError
```

**Solution:**
```javascript
'use strict';

const obj = { x: 10 };  // Not frozen
obj.x = 20;  // OK

// Or use let for non-frozen modification
let value = 10;
value = 20;  // OK
```

### Error 4: Octal Literals

**Problem:**
```javascript
'use strict';

const mode = 0755;  // SyntaxError
```

**Solution:**
```javascript
'use strict';

// Use decimal
const mode = 493;

// Or use binary (octal equivalent in JavaScript)
const mode = 0o755;
```

---

## 6. Strict Mode Analysis & Audit

### 6.1 Audit Script

**File: `audit-strict-mode.js`**

```javascript
'use strict';

import fs from 'fs';
import path from 'path';

const STRICT_MODE_PATTERN = /^[\s\n]*['"]use strict['"];/;

function checkFile(filePath) {
  const content = fs.readFileSync(filePath, 'utf-8');
  const hasStrictMode = STRICT_MODE_PATTERN.test(content);
  
  return {
    file: filePath,
    hasStrictMode,
    firstLine: content.split('\n')[0]
  };
}

function auditDirectory(dirPath, extensions = ['.js', '.mjs']) {
  const results = [];
  
  function traverse(dir) {
    const files = fs.readdirSync(dir);
    
    for (const file of files) {
      const fullPath = path.join(dir, file);
      const stat = fs.statSync(fullPath);
      
      if (stat.isDirectory() && !fullPath.includes('node_modules')) {
        traverse(fullPath);
      } else if (extensions.some(ext => file.endsWith(ext))) {
        results.push(checkFile(fullPath));
      }
    }
  }
  
  traverse(dirPath);
  return results;
}

// Run audit
const results = auditDirectory('./src');
const report = {
  total: results.length,
  withStrictMode: results.filter(r => r.hasStrictMode).length,
  withoutStrictMode: results.filter(r => !r.hasStrictMode).length,
  files: results
};

console.log(JSON.stringify(report, null, 2));
```

### 6.2 ESLint Strict Mode Rule

```javascript
// Custom ESLint rule configuration
module.exports = {
  rules: {
    'strict': [
      'error',
      'global'  // Enforce at global scope
    ]
  }
};

// Usage in .eslintrc.json
{
  "rules": {
    "strict": ["error", "global"]
  }
}
```

---

## 7. Integration with Development Workflow

### 7.1 Pre-commit Hook

**File: `.husky/pre-commit`**

```bash
#!/bin/sh
. "$(dirname "$0")/_/husky.sh"

npm run lint:strict
npm run test
```

### 7.2 GitHub Actions Workflow

**File: `.github/workflows/strict-mode-check.yml`**

```yaml
name: Strict Mode Check

on: [push, pull_request]

jobs:
  check-strict-mode:
    runs-on: ubuntu-latest
    
    steps:
      - uses: actions/checkout@v3
      
      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '18'
      
      - name: Install dependencies
        run: npm ci
      
      - name: Check strict mode
        run: npm run lint:strict
      
      - name: Run tests
        run: npm test
```

### 7.3 VSCode Settings

**File: `.vscode/settings.json`**

```json
{
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  "editor.formatOnSave": true,
  "eslint.enable": true,
  "eslint.run": "onSave",
  "[javascript]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": true
  }
}
```

---

## 8. Migration Guide: Non-Strict to Strict

### Phase 1: Audit
```bash
npm run audit:strict-mode
```

### Phase 2: Fix Issues Incrementally
```javascript
// Start with function-scoped strict mode
function legacyFunction() {
  'use strict';
  // Fix issues in this scope
}
```

### Phase 3: Expand Scope
```javascript
// Move to module-level
'use strict';

// All functions now strict
```

### Phase 4: Verify
```bash
npm run lint:strict
npm test
```

---

## 9. Performance Considerations

### 9.1 Runtime Performance

Strict mode has minimal performance impact:
- ~1-2% overhead in most cases
- Some optimizations are better with strict mode
- Modern engines handle it efficiently

### 9.2 Code Optimization

**Benefit:** Strict mode enables better optimizations:

```javascript
'use strict';

// Engine can optimize this better
function sum(a, b) {
  return a + b;  // No coercion concerns
}
```

---

## 10. Troubleshooting & FAQ

### Q: Can I mix strict and non-strict code?

**A:** Yes, but within limitations:
```javascript
'use strict';

// Strict function
function strict() { }

// Non-strict functions can't call strict safely
// Stick to one mode per project
```

### Q: Does strict mode break existing code?

**A:** Potentially. Always test thoroughly:
```bash
npm test  # Run full test suite before enabling globally
```

### Q: Are ES6 modules always strict?

**A:** Yes, modules are automatically in strict mode.

### Q: How do I handle third-party libraries?

**A:** Keep them in separate scopes or verify they're strict-compatible.

---

## 11. Checklist for Implementation

- [ ] Enable strict mode globally in package.json
- [ ] Configure ESLint with strict mode rules
- [ ] Add pre-commit hooks
- [ ] Setup GitHub Actions CI/CD
- [ ] Audit existing codebase
- [ ] Fix identified violations
- [ ] Document in README
- [ ] Train team on strict mode rules
- [ ] Test thoroughly
- [ ] Deploy with confidence

---

## 12. Resources & References

- [MDN: Strict Mode](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Strict_mode)
- [ECMAScript Specification](https://tc39.es/ecma262/)
- [ESLint Documentation](https://eslint.org/)
- [TypeScript Strict Mode](https://www.typescriptlang.org/tsconfig#strict)

---

## Document Metadata

- **Created:** 2025-12-21
- **Version:** 1.0.0
- **Author:** Cursor Rules Generator
- **Status:** Complete
- **Last Updated:** 2025-12-21 03:41:55 UTC
