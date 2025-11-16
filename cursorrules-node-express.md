# .cursorrules - Node.js/Express Backend APIs

**Version**: 1.0.0  
**Last Updated**: 2025-11-16  
**Context**: Backend APIs, Express, Databases  
**Tech Stack**: Node.js, Express.js, TypeScript, Database (PostgreSQL/MongoDB)

---

## Project Context
This project uses Node.js with Express.js for backend API development.
Focus areas: RESTful APIs, database operations, authentication, error handling, logging.

---

## Model Recommendations

### For This Context, Prefer:
- **Claude Sonnet 4.5** - Complex business logic, API design, security
- **Composer 1** - Multi-file refactoring, route organization
- **GPT-5.1 Codex** - Code generation, boilerplate creation
- **Haiku 4.5** - Quick iterations, simple endpoints

### Avoid:
- ❌ Fast/Low models for security-critical code
- ❌ Models without backend specialization for complex logic

### Budget-Friendly Options (Free/Unlimited):
- **Haiku 4.5** - Quick endpoints, simple CRUD operations (Unlimited, Very Fast) ⭐ **Best Budget Choice**
- **GPT-5 Fast** - Quick API endpoints, simple routes (Unlimited, Fast)
- **GPT-5.1 Fast** - Latest GPT with fast responses (Unlimited, Fast)
- **GPT-5 Codex Fast** - Quick code generation, simple edits (Unlimited, Fast)
- **GPT-5.1 Codex Fast** - Latest codex with speed (Unlimited, Fast)
- **GPT-5.1 Codex Low Fast** - Ultra-fast for rapid iterations (Unlimited, Very Fast)

**When to Use Budget Models:**
- ✅ Simple CRUD endpoints
- ✅ Quick iterations and experiments
- ✅ Basic route definitions
- ✅ Simple validation logic
- ✅ Non-critical endpoints
- ✅ Development/testing code

**When NOT to Use Budget Models:**
- ❌ Security-critical code (authentication, authorization)
- ❌ Complex business logic
- ❌ Database transaction handling
- ❌ Error handling architecture
- ❌ API design decisions

---

## Core Principles

### Node.js Best Practices
- **Async/Await** - Always use async/await, not callbacks
- **Error Handling** - Always wrap async operations in try-catch
- **Environment Variables** - Never hardcode secrets
- **Logging** - Use structured logging (Winston, Pino)
- **Validation** - Validate all inputs (Zod, Joi)

### Express.js Best Practices
- **Middleware Order** - Order matters (error handlers last)
- **Route Organization** - Group related routes
- **Request Validation** - Validate before processing
- **Response Format** - Consistent API response format
- **Error Handling** - Centralized error handling

### TypeScript Best Practices
- **Strict Mode** - Always use TypeScript strict mode
- **Type Safety** - Avoid `any`, use `unknown` when needed
- **Interface Definitions** - Define request/response types
- **Generic Types** - Use generics for reusable functions

---

## Code Style

### Naming Conventions
- **Files**: kebab-case (`user-controller.ts`, `auth-middleware.ts`)
- **Classes**: PascalCase (`UserController`, `AuthService`)
- **Functions**: camelCase (`getUserById`, `validateRequest`)
- **Variables**: camelCase (`userId`, `isAuthenticated`)
- **Constants**: UPPER_SNAKE_CASE (`MAX_RETRIES`, `API_BASE_URL`)
- **Types/Interfaces**: PascalCase (`UserRequest`, `ApiResponse`)

### File Organization
```
src/
├── routes/              # Route definitions
│   ├── users.ts
│   └── auth.ts
├── controllers/         # Request handlers
│   ├── user-controller.ts
│   └── auth-controller.ts
├── services/            # Business logic
│   ├── user-service.ts
│   └── auth-service.ts
├── middleware/          # Express middleware
│   ├── auth.ts
│   └── error-handler.ts
├── models/             # Database models
│   └── user.ts
├── types/              # TypeScript types
│   └── api.ts
├── utils/              # Utility functions
│   └── validation.ts
├── config/             # Configuration
│   └── database.ts
└── app.ts              # Express app setup
```

---

## Express Patterns

### Route Definition
```typescript
// ✅ GOOD: Organized route structure
import { Router } from 'express';
import { UserController } from '../controllers/user-controller';
import { authMiddleware } from '../middleware/auth';
import { validateRequest } from '../middleware/validation';

const router = Router();
const userController = new UserController();

router.get('/users', authMiddleware, userController.getAllUsers);
router.get('/users/:id', authMiddleware, validateRequest, userController.getUserById);
router.post('/users', authMiddleware, validateRequest, userController.createUser);
router.put('/users/:id', authMiddleware, validateRequest, userController.updateUser);
router.delete('/users/:id', authMiddleware, userController.deleteUser);

export default router;
```

### Controller Pattern
```typescript
// ✅ GOOD: Controller with proper error handling
import { Request, Response, NextFunction } from 'express';
import { UserService } from '../services/user-service';
import { logger } from '../utils/logger';

export class UserController {
  private userService = new UserService();

  async getAllUsers(req: Request, res: Response, next: NextFunction) {
    try {
      const users = await this.userService.getAllUsers();
      res.json({ success: true, data: users });
    } catch (error) {
      logger.error('Failed to get users', { error });
      next(error);
    }
  }

  async getUserById(req: Request, res: Response, next: NextFunction) {
    try {
      const { id } = req.params;
      const user = await this.userService.getUserById(id);
      
      if (!user) {
        return res.status(404).json({ success: false, error: 'User not found' });
      }
      
      res.json({ success: true, data: user });
    } catch (error) {
      logger.error('Failed to get user', { error, userId: req.params.id });
      next(error);
    }
  }
}
```

### Service Pattern
```typescript
// ✅ GOOD: Service layer for business logic
import { UserRepository } from '../repositories/user-repository';
import { logger } from '../utils/logger';

export class UserService {
  private userRepository = new UserRepository();

  async getAllUsers() {
    try {
      return await this.userRepository.findAll();
    } catch (error) {
      logger.error('UserService: Failed to get all users', { error });
      throw error;
    }
  }

  async getUserById(id: string) {
    try {
      const user = await this.userRepository.findById(id);
      return user;
    } catch (error) {
      logger.error('UserService: Failed to get user', { error, userId: id });
      throw error;
    }
  }
}
```

---

## Error Handling

### Centralized Error Handler
```typescript
// ✅ GOOD: Centralized error handling middleware
import { Request, Response, NextFunction } from 'express';
import { logger } from '../utils/logger';

export function errorHandler(
  error: Error,
  req: Request,
  res: Response,
  next: NextFunction
) {
  logger.error('Request failed', {
    error: error.message,
    stack: error.stack,
    path: req.path,
    method: req.method,
  });

  // Don't expose internal errors in production
  const message = process.env.NODE_ENV === 'production' 
    ? 'Internal server error' 
    : error.message;

  res.status(500).json({
    success: false,
    error: message,
  });
}
```

### Custom Error Classes
```typescript
// ✅ GOOD: Custom error classes
export class NotFoundError extends Error {
  constructor(message: string) {
    super(message);
    this.name = 'NotFoundError';
  }
}

export class ValidationError extends Error {
  constructor(message: string, public details?: unknown) {
    super(message);
    this.name = 'ValidationError';
  }
}

// Usage
if (!user) {
  throw new NotFoundError('User not found');
}
```

---

## Validation

### Zod Schema Validation
```typescript
// ✅ GOOD: Zod validation
import { z } from 'zod';

const createUserSchema = z.object({
  name: z.string().min(1, 'Name is required'),
  email: z.string().email('Invalid email format'),
  age: z.number().int().min(18, 'Must be 18 or older'),
});

export function validateCreateUser(req: Request, res: Response, next: NextFunction) {
  try {
    const validated = createUserSchema.parse(req.body);
    req.body = validated; // Replace with validated data
    next();
  } catch (error) {
    if (error instanceof z.ZodError) {
      return res.status(400).json({
        success: false,
        error: 'Validation failed',
        details: error.errors,
      });
    }
    next(error);
  }
}
```

---

## Database Operations

### Repository Pattern
```typescript
// ✅ GOOD: Repository pattern for database access
import { db } from '../config/database';

export class UserRepository {
  async findAll() {
    try {
      const result = await db.query('SELECT * FROM users');
      return result.rows;
    } catch (error) {
      throw new Error(`Failed to fetch users: ${error}`);
    }
  }

  async findById(id: string) {
    try {
      const result = await db.query('SELECT * FROM users WHERE id = $1', [id]);
      return result.rows[0] || null;
    } catch (error) {
      throw new Error(`Failed to fetch user: ${error}`);
    }
  }

  async create(userData: CreateUserData) {
    try {
      const result = await db.query(
        'INSERT INTO users (name, email) VALUES ($1, $2) RETURNING *',
        [userData.name, userData.email]
      );
      return result.rows[0];
    } catch (error) {
      throw new Error(`Failed to create user: ${error}`);
    }
  }
}
```

### Transaction Handling
```typescript
// ✅ GOOD: Transaction handling
async function transferMoney(fromId: string, toId: string, amount: number) {
  const client = await db.getClient();
  
  try {
    await client.query('BEGIN');
    
    // Deduct from sender
    await client.query(
      'UPDATE accounts SET balance = balance - $1 WHERE id = $2',
      [amount, fromId]
    );
    
    // Add to receiver
    await client.query(
      'UPDATE accounts SET balance = balance + $1 WHERE id = $2',
      [amount, toId]
    );
    
    await client.query('COMMIT');
  } catch (error) {
    await client.query('ROLLBACK');
    throw error;
  } finally {
    client.release();
  }
}
```

---

## Authentication & Security

### JWT Authentication
```typescript
// ✅ GOOD: JWT authentication middleware
import jwt from 'jsonwebtoken';
import { Request, Response, NextFunction } from 'express';
import { logger } from '../utils/logger';

export function authMiddleware(req: Request, res: Response, next: NextFunction) {
  try {
    const token = req.headers.authorization?.replace('Bearer ', '');
    
    if (!token) {
      return res.status(401).json({ success: false, error: 'No token provided' });
    }

    const secret = process.env.JWT_SECRET;
    if (!secret) {
      // This check is ideally done once at application startup.
      logger.error('FATAL ERROR: JWT_SECRET is not defined.');
      return res.status(500).json({ success: false, error: 'Internal server error' });
    }

    const decoded = jwt.verify(token, secret) as { userId: string };
    req.userId = decoded.userId;
    next();
  } catch (error) {
    return res.status(401).json({ success: false, error: 'Invalid token' });
  }
}
```

### Password Hashing
```typescript
// ✅ GOOD: Password hashing with bcrypt
import bcrypt from 'bcrypt';

export async function hashPassword(password: string): Promise<string> {
  const saltRounds = 10;
  return await bcrypt.hash(password, saltRounds);
}

export async function verifyPassword(
  password: string,
  hash: string
): Promise<boolean> {
  return await bcrypt.compare(password, hash);
}
```

---

## Logging

### Structured Logging
```typescript
// ✅ GOOD: Winston structured logging
import winston from 'winston';

export const logger = winston.createLogger({
  level: process.env.LOG_LEVEL || 'info',
  format: winston.format.combine(
    winston.format.timestamp(),
    winston.format.errors({ stack: true }),
    winston.format.json()
  ),
  transports: [
    new winston.transports.File({ filename: 'logs/error.log', level: 'error' }),
    new winston.transports.File({ filename: 'logs/combined.log' }),
  ],
});

// Usage
logger.info('User created', { userId: user.id, email: user.email });
logger.error('Database error', { error: error.message, stack: error.stack });
```

---

## API Response Format

### Consistent Response Structure
```typescript
// ✅ GOOD: Consistent API response format
interface ApiResponse<T> {
  success: boolean;
  data?: T;
  error?: string;
  details?: unknown;
}

// Success response
res.json({ success: true, data: users });

// Error response
res.status(400).json({
  success: false,
  error: 'Validation failed',
  details: validationErrors,
});
```

---

## Testing

### API Testing
```typescript
// ✅ GOOD: Supertest for API testing
import request from 'supertest';
import { app } from '../app';

describe('GET /users', () => {
  it('should return all users', async () => {
    const response = await request(app)
      .get('/users')
      .set('Authorization', `Bearer ${token}`)
      .expect(200);

    expect(response.body.success).toBe(true);
    expect(Array.isArray(response.body.data)).toBe(true);
  });
});
```

---

## Anti-Patterns to Avoid

❌ **No Error Handling**
```typescript
// BAD
app.get('/users', async (req, res) => {
  const users = await db.query('SELECT * FROM users');
  res.json(users); // No error handling!
});
```

❌ **Hardcoded Secrets**
```typescript
// BAD
const secret = 'my-secret-key'; // NO!
```

❌ **SQL Injection**
```typescript
// BAD
const query = `SELECT * FROM users WHERE id = ${userId}`; // NO!
```

❌ **No Input Validation**
```typescript
// BAD
app.post('/users', async (req, res) => {
  const user = await createUser(req.body); // No validation!
});
```

---

## Model Selection Guide

| Task Type | Recommended Model | Budget Alternative | Reason |
|-----------|-------------------|-------------------|--------|
| **API Design** | Claude Sonnet 4.5 | Haiku 4.5 (simple) | Best reasoning for architecture |
| **Multi-file Refactoring** | Composer 1 | GPT-5.1 Codex Fast | Efficient for structured changes |
| **Security Implementation** | Claude Opus 4.1 | Claude Sonnet 4.5 | Mission-critical |
| **Code Generation** | GPT-5.1 Codex | GPT-5.1 Codex Fast | Specialized for code |
| **Quick Endpoints** | Haiku 4.5 | GPT-5.1 Codex Low Fast | Fast, good enough for simple APIs |
| **Complex Business Logic** | Claude Sonnet 4.5 | Haiku 4.5 (simple) | Best reasoning |
| **Simple CRUD** | Haiku 4.5 | GPT-5.1 Codex Low Fast | Best budget choice |
| **Basic Routes** | Haiku 4.5 | GPT-5 Fast | Route definitions |

---

**Remember**: Backend code handles user data and business logic. Always prioritize security, error handling, and data validation.

