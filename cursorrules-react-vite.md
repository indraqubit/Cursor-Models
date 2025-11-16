# .cursorrules - React/Vite Modern Web Apps

**Version**: 1.0.0  
**Last Updated**: 2025-11-16  
**Context**: Modern Web Apps with React, Vite, TypeScript  
**Tech Stack**: React 18, Vite, TypeScript, TailwindCSS, Modern Web Development

---

## Project Context
This project uses React 18 with Vite for modern web application development.
Focus areas: Component architecture, state management, performance optimization, TypeScript type safety.

---

## Model Recommendations

### For This Context, Prefer:
- **Composer 1** - Multi-file component refactoring, feature implementation
- **Claude Sonnet 4.5** - Complex state management, architecture decisions
- **Haiku 4.5** - Quick iterations, autocomplete, simple components
- **GPT-5.1 Codex** - Code generation, boilerplate creation

### Avoid:
- ❌ Heavy models for simple UI changes (use Haiku)
- ❌ Slow models for quick iterations (use Fast variants)

### Budget-Friendly Options (Free/Unlimited):
- **Haiku 4.5** - Quick iterations, autocomplete, simple components (Unlimited, Very Fast) ⭐ **Best Budget Choice**
- **GPT-5 Fast** - Quick component creation, simple refactoring (Unlimited, Fast)
- **GPT-5.1 Fast** - Latest GPT with fast responses (Unlimited, Fast)
- **GPT-5 Codex Fast** - Quick code generation, simple edits (Unlimited, Fast)
- **GPT-5.1 Codex Fast** - Latest codex with speed (Unlimited, Fast)
- **GPT-5.1 Codex Low Fast** - Ultra-fast for rapid iterations (Unlimited, Very Fast)

**When to Use Budget Models:**
- ✅ Simple UI components
- ✅ Quick iterations and experiments
- ✅ TypeScript type fixes
- ✅ Styling and CSS changes
- ✅ Component props updates
- ✅ Simple state management

**When NOT to Use Budget Models:**
- ❌ Complex state management logic
- ❌ Architecture decisions
- ❌ Multi-file refactoring (use Composer 1)
- ❌ Performance-critical optimizations

---

## Core Principles

### React Best Practices
- **Functional Components** - Always use function components with hooks
- **Hooks Rules** - Only call hooks at top level, not in loops/conditions
- **Component Composition** - Prefer composition over inheritance
- **Single Responsibility** - One component = one purpose
- **Props Immutability** - Never mutate props directly

### TypeScript Best Practices
- **Strict Mode** - Always use TypeScript strict mode
- **Type Safety** - Avoid `any`, use `unknown` when needed
- **Type Inference** - Let TypeScript infer when possible
- **Interface vs Type** - Prefer `type` for unions, `interface` for objects
- **Generic Types** - Use generics for reusable components

### Vite Best Practices
- **ESM First** - Use ES modules, not CommonJS
- **Tree Shaking** - Import only what you need
- **Code Splitting** - Use dynamic imports for routes
- **Asset Optimization** - Use Vite's asset handling
- **Hot Module Replacement** - Leverage HMR for fast development

---

## Code Style

### Naming Conventions
- **Components**: PascalCase (`UserProfile`, `ProductCard`)
- **Hooks**: camelCase with `use` prefix (`useAuth`, `useLocalStorage`)
- **Functions**: camelCase (`handleSubmit`, `fetchUserData`)
- **Variables**: camelCase (`userName`, `isLoading`)
- **Constants**: UPPER_SNAKE_CASE (`API_BASE_URL`, `MAX_RETRIES`)
- **Types/Interfaces**: PascalCase (`UserData`, `ApiResponse`)
- **Files**: kebab-case (`user-profile.tsx`, `api-client.ts`)

### File Organization
```
src/
├── components/
│   ├── common/          # Shared components
│   ├── ui/              # UI primitives (shadcn/ui)
│   └── features/        # Feature-specific components
├── features/            # Feature-first organization
│   ├── auth/
│   ├── dashboard/
│   └── settings/
├── hooks/               # Custom React hooks
├── lib/
│   ├── api/            # API client
│   ├── utils/         # Utility functions
│   └── config/        # Configuration
├── types/              # TypeScript types
├── App.tsx
└── main.tsx
```

---

## Component Patterns

### Functional Component Template
```tsx
// ✅ GOOD: Proper component structure
import { useState, useEffect } from 'react';
import type { UserProfileProps } from './types';

/**
 * UserProfile component displays user information
 * 
 * @param user - User data to display
 * @param onEdit - Callback when edit button is clicked
 */
export function UserProfile({ user, onEdit }: UserProfileProps) {
  const [isLoading, setIsLoading] = useState(false);

  useEffect(() => {
    // Side effects here
  }, [user]);

  const handleEdit = () => {
    setIsLoading(true);
    onEdit?.();
  };

  if (isLoading) {
    return <div>Loading...</div>;
  }

  return (
    <div className="user-profile">
      {/* Component JSX */}
    </div>
  );
}
```

### Custom Hooks Pattern
```tsx
// ✅ GOOD: Custom hook for reusable logic
export function useAuth() {
  const [user, setUser] = useState<User | null>(null);
  const [isLoading, setIsLoading] = useState(true);

  useEffect(() => {
    // Fetch user data
    fetchUser().then(setUser).finally(() => setIsLoading(false));
  }, []);

  return { user, isLoading };
}
```

---

## State Management

### Local State (useState)
```tsx
// ✅ GOOD: Simple local state
const [count, setCount] = useState(0);
const [user, setUser] = useState<User | null>(null);
```

### Derived State (useMemo)
```tsx
// ✅ GOOD: Memoized derived state
const filteredUsers = useMemo(() => {
  return users.filter(user => user.active);
}, [users]);
```

### Context API (useContext)
```tsx
// ✅ GOOD: Context for shared state
const AuthContext = createContext<AuthContextType | null>(null);

export function AuthProvider({ children }: { children: React.ReactNode }) {
  const [user, setUser] = useState<User | null>(null);
  return (
    <AuthContext.Provider value={{ user, setUser }}>
      {children}
    </AuthContext.Provider>
  );
}
```

### External State (Zustand/Redux)
```tsx
// ✅ GOOD: Zustand for complex state
import { create } from 'zustand';

interface UserStore {
  user: User | null;
  setUser: (user: User) => void;
}

export const useUserStore = create<UserStore>((set) => ({
  user: null,
  setUser: (user) => set({ user }),
}));
```

---

## TypeScript Patterns

### Component Props
```tsx
// ✅ GOOD: Proper prop types
interface ButtonProps {
  label: string;
  onClick: () => void;
  variant?: 'primary' | 'secondary';
  disabled?: boolean;
}

export function Button({ label, onClick, variant = 'primary', disabled }: ButtonProps) {
  // Component implementation
}
```

### API Types
```tsx
// ✅ GOOD: API response types
type ApiResponse<T> = {
  success: boolean;
  data: T;
  error?: string;
};

type User = {
  id: string;
  name: string;
  email: string;
};
```

### Generic Components
```tsx
// ✅ GOOD: Generic list component
interface ListProps<T> {
  items: T[];
  renderItem: (item: T) => React.ReactNode;
}

export function List<T>({ items, renderItem }: ListProps<T>) {
  return (
    <ul>
      {items.map((item, index) => (
        <li key={index}>{renderItem(item)}</li>
      ))}
    </ul>
  );
}
```

---

## Performance Optimization

### React.memo
```tsx
// ✅ GOOD: Memoize expensive components
export const ExpensiveComponent = React.memo(({ data }: Props) => {
  // Expensive rendering
});
```

### useCallback
```tsx
// ✅ GOOD: Memoize callbacks
const handleClick = useCallback(() => {
  doSomething();
}, [dependency]);
```

### useMemo
```tsx
// ✅ GOOD: Memoize expensive computations
const expensiveValue = useMemo(() => {
  return computeExpensiveValue(data);
}, [data]);
```

### Code Splitting
```tsx
// ✅ GOOD: Lazy load routes
const Dashboard = lazy(() => import('./features/dashboard/Dashboard'));

function App() {
  return (
    <Suspense fallback={<Loading />}>
      <Routes>
        <Route path="/dashboard" element={<Dashboard />} />
      </Routes>
    </Suspense>
  );
}
```

---

## Vite Configuration

### vite.config.ts
```typescript
// ✅ GOOD: Optimized Vite config
import { defineConfig } from 'vite';
import react from '@vitejs/plugin-react';
import path from 'path';

export default defineConfig({
  plugins: [react()],
  resolve: {
    alias: {
      '@': path.resolve(__dirname, './src'),
    },
  },
  build: {
    rollupOptions: {
      output: {
        manualChunks: {
          vendor: ['react', 'react-dom'],
          router: ['react-router-dom'],
        },
      },
    },
  },
});
```

---

## Error Handling

### Error Boundaries
```tsx
// ✅ GOOD: Error boundary component
class ErrorBoundary extends React.Component {
  state = { hasError: false };

  static getDerivedStateFromError(error: Error) {
    return { hasError: true };
  }

  componentDidCatch(error: Error, errorInfo: React.ErrorInfo) {
    console.error('Error caught:', error, errorInfo);
  }

  render() {
    if (this.state.hasError) {
      return <ErrorFallback />;
    }
    return this.props.children;
  }
}
```

### Async Error Handling
```tsx
// ✅ GOOD: Proper async error handling
const fetchData = async () => {
  try {
    const response = await fetch('/api/data');
    if (!response.ok) {
      throw new Error(`HTTP ${response.status}: ${response.statusText}`);
    }
    return await response.json();
  } catch (error) {
    console.error('Fetch failed:', error);
    throw error;
  }
};
```

---

## Testing

### Component Testing
```tsx
// ✅ GOOD: React Testing Library
import { render, screen } from '@testing-library/react';
import { UserProfile } from './UserProfile';

test('renders user name', () => {
  render(<UserProfile user={{ name: 'John' }} />);
  expect(screen.getByText('John')).toBeInTheDocument();
});
```

---

## Styling

### TailwindCSS
```tsx
// ✅ GOOD: TailwindCSS classes
<div className="flex items-center justify-between p-4 bg-white rounded-lg shadow-md">
  <h2 className="text-xl font-bold text-gray-800">Title</h2>
</div>
```

### CSS Modules
```tsx
// ✅ GOOD: CSS Modules
import styles from './UserProfile.module.css';

<div className={styles.container}>
  <h2 className={styles.title}>User Profile</h2>
</div>
```

---

## Anti-Patterns to Avoid

❌ **Mutating State Directly**
```tsx
// BAD
const [users, setUsers] = useState([]);
users.push(newUser); // NO!
```

❌ **Missing Dependencies in useEffect**
```tsx
// BAD
useEffect(() => {
  fetchData(userId);
}, []); // Missing userId dependency
```

❌ **Using `any` Type**
```tsx
// BAD
const data: any = fetchData(); // NO!
```

❌ **Inline Functions in JSX**
```tsx
// BAD
<button onClick={() => handleClick(id)}>Click</button> // Creates new function each render
```

---

## Model Selection Guide

| Task Type | Recommended Model | Budget Alternative | Reason |
|-----------|-------------------|-------------------|--------|
| **Multi-file Refactoring** | Composer 1 | GPT-5.1 Codex Fast | Best for structured changes |
| **Component Creation** | Haiku 4.5 | GPT-5.1 Codex Low Fast | Fast, good enough for UI |
| **Complex State Logic** | Claude Sonnet 4.5 | Haiku 4.5 (simple) | Best reasoning |
| **TypeScript Types** | Composer 1 | GPT-5.1 Codex Fast | Efficient for type updates |
| **Quick Iterations** | Haiku 4.5 | GPT-5.1 Codex Low Fast | Very fast |
| **Architecture Decisions** | Claude Opus 4.1 | Claude Sonnet 4.5 | Mission-critical |
| **Simple UI Components** | Haiku 4.5 | GPT-5.1 Codex Low Fast | Best budget choice |
| **Styling Changes** | Haiku 4.5 | GPT-5 Low Fast | CSS/Tailwind updates |

---

**Remember**: React is about components and composition. Keep components small, focused, and reusable.

