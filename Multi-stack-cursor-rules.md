# .cursorrules untuk React+Vite+JUCE Stack

## Root Project `.cursorrules`

```json
{
  "project": {
    "name": "Audio Plugin + Dashboard",
    "stack": ["C++17", "JUCE 8", "React", "Vite", "TypeScript"],
    "architecture": "monorepo"
  },
  
  "general": {
    "style": "DRY, brutal & ringkas",
    "approach": "vibe coding → cleanup later",
    "shipping": "MVP first, perfect never",
    "communication": "Indonesian + English tech terms"
  },
  
  "cpp": {
    "standard": "C++17",
    "framework": "JUCE 8",
    "naming": {
      "classes": "PascalCase",
      "methods": "camelCase",
      "members": "camelCase",
      "constants": "SCREAMING_SNAKE_CASE"
    },
    "audio": {
      "realtime_safe": true,
      "no_allocations": "in processBlock/getNextAudioBlock",
      "thread_safety": "enforce with juce::CriticalSection",
      "buffer_size": "prepare for 32-2048 samples"
    },
    "principles": [
      "No new/delete in audio thread",
      "Use juce::AudioBuffer, avoid raw pointers",
      "Atomic operations for parameter changes",
      "Lock-free where possible"
    ]
  },
  
  "react": {
    "version": "18+",
    "bundler": "Vite 5",
    "language": "TypeScript strict mode",
    "structure": {
      "components": "src/components/{feature}/ComponentName.tsx",
      "hooks": "src/hooks/use{Feature}.ts",
      "utils": "src/utils/{category}.ts",
      "types": "src/types/{feature}.ts"
    },
    "style": {
      "css": "Tailwind CSS v4",
      "components": "shadcn/ui preferred",
      "responsive": "mobile-first"
    },
    "state": {
      "local": "useState/useReducer",
      "global": "Zustand (lightweight) or Context",
      "server": "TanStack Query for API"
    },
    "principles": [
      "Functional components only",
      "Custom hooks for reusable logic",
      "Avoid prop drilling > 2 levels",
      "Memoize expensive computations"
    ]
  },
  
  "api": {
    "convention": "RESTful",
    "format": "JSON",
    "auth": "JWT (juwita-sdk style)",
    "errors": {
      "format": "{ error: string, code: number, details?: any }",
      "status_codes": "Standard HTTP"
    }
  },
  
  "testing": {
    "cpp": {
      "framework": "JUCE UnitTest",
      "run": "after each audio logic change",
      "focus": ["DSP algorithms", "parameter ranges", "thread safety"]
    },
    "react": {
      "framework": "Vitest + Testing Library",
      "run": "before commit",
      "focus": ["user interactions", "API integration", "edge cases"]
    }
  },
  
  "workflow": {
    "cursor_composer": {
      "use_for": ["feature implementation", "refactoring", "bug fixes"],
      "context": "Always include relevant files in chat"
    },
    "cline": {
      "use_for": ["testing", "benchmarking", "deployment scripts"],
      "terminal": "Keep separate for long-running tasks"
    },
    "copilot": {
      "use_for": ["boilerplate", "repetitive code", "common patterns"],
      "accept": "Review before accepting suggestions"
    }
  },
  
  "performance": {
    "cpp": [
      "Profile with Instruments/VTune before optimizing",
      "Target: <1ms processBlock at 512 samples/44.1kHz",
      "Avoid: std::vector resize, string operations in RT thread"
    ],
    "react": [
      "Lazy load routes with React.lazy",
      "Virtualize long lists (react-window)",
      "Debounce frequent updates (audio meters)"
    ]
  },
  
  "security": {
    "license": "Validate JWT signatures (juwita-sdk)",
    "api": "Rate limit, input validation, CORS",
    "storage": "Never store sensitive data in localStorage"
  },
  
  "git": {
    "commits": "Conventional Commits (feat:, fix:, refactor:)",
    "branches": "feature/*, bugfix/*, hotfix/*",
    "pre_commit": ["lint", "format", "test"]
  }
}
```

---

## Vite-Specific `.cursorrules` (Frontend folder)

```json
{
  "vite": {
    "version": "5+",
    "plugins": ["@vitejs/plugin-react-swc", "vite-plugin-pwa"],
    "config": {
      "optimization": {
        "splitChunks": "vendor, components, utils",
        "minify": "esbuild in production"
      },
      "dev": {
        "port": 3000,
        "proxy": {
          "/api": "http://localhost:8080"
        }
      }
    }
  },
  
  "imports": {
    "aliases": {
      "@/components": "./src/components",
      "@/hooks": "./src/hooks",
      "@/utils": "./src/utils",
      "@/types": "./src/types",
      "@/api": "./src/api"
    },
    "prefer": "named imports over default",
    "sorting": "React imports first, then third-party, then local"
  },
  
  "typescript": {
    "strict": true,
    "rules": [
      "No 'any' type without // @ts-expect-error comment",
      "Interfaces for public APIs, types for internal",
      "Use 'unknown' over 'any' when type unclear"
    ]
  },
  
  "components": {
    "template": "
      import { FC } from 'react';
      
      interface {Name}Props {
        // props here
      }
      
      export const {Name}: FC<{Name}Props> = ({ }) => {
        return (
          <div>
            {/* component */}
          </div>
        );
      };
    "
  }
}
```

---

## Cursor Tasks (MCP-style automation)

```json
{
  "tasks": {
    "test_audio": {
      "command": "cd juce && cmake --build build --target RunTests",
      "trigger": "after C++ file save in Source/"
    },
    
    "benchmark": {
      "command": "cd juce && ./build/Benchmark --samples 512",
      "trigger": "manual"
    },
    
    "test_react": {
      "command": "cd frontend && npm run test",
      "trigger": "before commit"
    },
    
    "dev_full_stack": {
      "command": "concurrently 'npm run dev:api' 'npm run dev:frontend'",
      "trigger": "manual"
    },
    
    "build_plugin": {
      "command": "cd juce && cmake --build build --config Release",
      "trigger": "manual"
    },
    
    "export_onnx": {
      "command": "python scripts/export_model.py --format onnx",
      "trigger": "after model training"
    }
  }
}
```

---

## Project Structure

```
audio-plugin-project/
├── .cursorrules               # Root config (this file)
├── juce/
│   ├── .cursorrules          # C++ specific
│   ├── Source/
│   └── CMakeLists.txt
├── frontend/
│   ├── .cursorrules          # React specific
│   ├── src/
│   ├── vite.config.ts
│   └── package.json
├── api/
│   ├── server.ts             # Node/Express or Python/FastAPI
│   └── routes/
└── scripts/
    ├── export_model.py
    └── deploy.sh
```

---

## Cline Usage Pattern

```bash
# Terminal 1: Cline for testing
cline "Watch JUCE tests, re-run on file change"

# Terminal 2: Cline for benchmarking
cline "Benchmark processBlock every 5 mins, log to metrics.json"

# Terminal 3: Dev servers
npm run dev:full-stack
```

---

## Copilot Triggers

```cpp
// JUCE: Type comment, Copilot generates
// "Create normalized parameter 0-1 mapped to dB -60 to +12"

// React: Type component name
// AudioMeterComponent → Copilot suggests full implementation
```

---

**Workflow:**
1. **Cursor Composer** - Main development, context-aware
2. **Cline** - Automated testing/benchmarking in background
3. **Copilot** - Quick boilerplate/common patterns
4. **Rules enforce** - Consistency across tools

Ship it! 🚀
