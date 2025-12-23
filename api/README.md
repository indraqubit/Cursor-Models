# API Directory

This directory contains the backend API server files.

## Structure

```
api/
├── server.ts           # Main server entry point
└── routes/             # API route handlers
    ├── auth.ts
    ├── plugins.ts
    └── models.ts
```

## Guidelines

- Follow RESTful conventions
- Use JWT authentication (juwita-sdk style)
- Implement rate limiting
- Validate all inputs
- Use standard HTTP status codes
- Error format: `{ error: string, code: number, details?: any }`
