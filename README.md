# Cursor AI Model Selection Templates

<div align="center">

![Version](https://img.shields.io/badge/version-1.0.0-blue.svg)
![License](https://img.shields.io/badge/license-MIT-green.svg)
![Last Updated](https://img.shields.io/badge/last%20updated-2025--01--16-lightgrey.svg)
![GitHub](https://img.shields.io/github/stars/indraqubit/Cursor-Models?style=social)
![GitHub forks](https://img.shields.io/github/forks/indraqubit/Cursor-Models?style=social)

[![Cursor AI](https://img.shields.io/badge/Cursor-AI%20Models-purple.svg)](https://cursor.sh)
[![Documentation](https://img.shields.io/badge/docs-complete-brightgreen.svg)](./Cursor%20AI%20Models%20-%20Complete%20Reference.md)
[![Changelog](https://img.shields.io/badge/changelog-available-orange.svg)](./CHANGELOG.md)

**Context-specific `.cursorrules` templates for optimal AI model selection**

[📋 Changelog](./CHANGELOG.md) • [📖 Complete Reference](./Cursor%20AI%20Models%20-%20Complete%20Reference.md) • [🚀 Quick Start](#-how-to-use)

</div>

---

## 📋 Overview

This directory contains `.cursorrules` templates optimized for different development contexts. Each template includes:

- **Model Recommendations** - Best AI models for specific tasks
- **Tech Stack Guidelines** - Best practices for the technology
- **Code Patterns** - Common patterns and examples
- **Anti-Patterns** - What to avoid
- **Model Selection Guide** - When to use which model

### 🤖 Supported AI Models

![Claude](https://img.shields.io/badge/Claude-Sonnet%204.5-orange.svg)
![Claude](https://img.shields.io/badge/Claude-Opus%204.1-orange.svg)
![GPT](https://img.shields.io/badge/GPT-5.1%20Codex-green.svg)
![Composer](https://img.shields.io/badge/Composer-1.0-purple.svg)
![Haiku](https://img.shields.io/badge/Haiku-4.5-blue.svg)
![Gemini](https://img.shields.io/badge/Gemini-2.5%20Pro-yellow.svg)
![Grok](https://img.shields.io/badge/Grok-4-red.svg)
![DeepSeek](https://img.shields.io/badge/DeepSeek-V3.1-lightblue.svg)
![Kimi](https://img.shields.io/badge/Kimi-K2-pink.svg)

**See [Complete Model Reference](./Cursor%20AI%20Models%20-%20Complete%20Reference.md) for full list**

---

## 📁 Available Templates

### 1. **C++/JUCE 8 Audio Plugins** (`cursorrules-cpp-juce.md`)

![C++](https://img.shields.io/badge/C++-17/20-blue.svg)
![JUCE](https://img.shields.io/badge/JUCE-8.0-orange.svg)
![DSP](https://img.shields.io/badge/DSP-Audio%20Processing-purple.svg)

**Context**: Audio Plugins, DSP, Real-time Systems  
**Tech Stack**: C++17-20, JUCE 8

**Best For:**
- Audio plugin development
- DSP algorithm implementation
- Real-time audio processing
- Low-latency systems

**Recommended Models:**
- Claude Sonnet 4.5 - Complex DSP logic
- Composer 1 - Multi-file refactoring
- GPT-5.1 Codex - Code generation
- **Budget**: Haiku 4.5, GPT-5.1 Codex Fast (Unlimited)

---

### 2. **React/Vite Modern Web Apps** (`cursorrules-react-vite.md`)

![React](https://img.shields.io/badge/React-18-blue.svg)
![Vite](https://img.shields.io/badge/Vite-Latest-yellow.svg)
![TypeScript](https://img.shields.io/badge/TypeScript-5.0-blue.svg)
![TailwindCSS](https://img.shields.io/badge/TailwindCSS-3.0-38bdf8.svg)

**Context**: Modern Web Apps with React, Vite, TypeScript  
**Tech Stack**: React 18, Vite, TypeScript, TailwindCSS

**Best For:**
- React component development
- Modern web applications
- TypeScript frontend projects
- Vite-based builds

**Recommended Models:**
- Composer 1 - Multi-file refactoring
- Claude Sonnet 4.5 - Complex state management
- Haiku 4.5 - Quick iterations ⭐ **Best Budget Choice**
- **Budget**: GPT-5.1 Codex Fast, GPT-5.1 Codex Low Fast (Unlimited)

---

### 3. **Node.js/Express Backend APIs** (`cursorrules-node-express.md`)

![Node.js](https://img.shields.io/badge/Node.js-20+-green.svg)
![Express](https://img.shields.io/badge/Express-4.x-lightgrey.svg)
![TypeScript](https://img.shields.io/badge/TypeScript-5.0-blue.svg)
![API](https://img.shields.io/badge/API-RESTful-red.svg)

**Context**: Backend APIs, Express, Databases  
**Tech Stack**: Node.js, Express.js, TypeScript

**Best For:**
- RESTful API development
- Backend services
- Database operations
- Authentication & authorization

**Recommended Models:**
- Claude Sonnet 4.5 - Complex business logic
- Composer 1 - Multi-file refactoring
- GPT-5.1 Codex - Code generation
- **Budget**: Haiku 4.5, GPT-5.1 Codex Fast (Unlimited) ⭐ **Best Budget Choice**

---

### 4. **Scripting & Automation** (`cursorrules-scripting.md`)

![Python](https://img.shields.io/badge/Python-3.x-blue.svg)
![Shell](https://img.shields.io/badge/Shell-Bash/Zsh-green.svg)
![Node.js](https://img.shields.io/badge/Node.js-Scripts-green.svg)
![DevOps](https://img.shields.io/badge/DevOps-Automation-orange.svg)

**Context**: Scripting, Automation, DevOps, Utilities  
**Tech Stack**: Python, Shell (Bash/Zsh), Node.js scripts

**Best For:**
- Automation scripts
- DevOps tasks
- Utility scripts
- Build/deployment scripts

**Recommended Models:**
- Haiku 4.5 - Quick scripts ⭐ **Best Budget Choice**
- GPT-5.1 Codex - Code generation
- Claude Sonnet 4.5 - Complex automation
- **Budget**: GPT-5.1 Codex Fast, GPT-5 Low Fast (Unlimited)

---

## 🚀 How to Use

### Step 1: Copy Template to Your Project

```bash
# For C++/JUCE project
cp cursorrules-cpp-juce.md /path/to/your/audio-plugin-project/.cursorrules

# For React/Vite project
cp cursorrules-react-vite.md /path/to/your/web-app/.cursorrules

# For Node.js/Express project
cp cursorrules-node-express.md /path/to/your/backend-api/.cursorrules

# For scripting project
cp cursorrules-scripting.md /path/to/your/scripts/.cursorrules
```

### Step 2: Customize for Your Project

Edit the `.cursorrules` file to match your specific:
- Project structure
- Tech stack versions
- Team conventions
- Specific requirements

### Step 3: Use Folder-Specific Rules (Optional)

You can have multiple `.cursorrules` files in different folders:

```
my-monorepo/
├── .cursorrules                    # Global defaults
├── audio-plugins/
│   └── .cursorrules                # C++/JUCE specific
├── web-app/
│   └── .cursorrules                # React/Vite specific
└── backend-api/
    └── .cursorrules                # Node.js/Express specific
```

**Cursor will automatically use the closest `.cursorrules` file to the file you're editing.**

---

## 🎯 Model Selection Strategy

### Quick Reference

| Context | Primary Model | Secondary Model | Budget Model (Free) |
|---------|--------------|-----------------|---------------------|
| **C++/JUCE** | Claude Sonnet 4.5 | Composer 1 | Haiku 4.5, GPT-5.1 Codex Fast |
| **React/Vite** | Composer 1 | Claude Sonnet 4.5 | Haiku 4.5 ⭐, GPT-5.1 Codex Low Fast |
| **Node.js/Express** | Claude Sonnet 4.5 | Composer 1 | Haiku 4.5 ⭐, GPT-5.1 Codex Fast |
| **Scripting** | Haiku 4.5 ⭐ | GPT-5.1 Codex | GPT-5.1 Codex Low Fast, GPT-5 Low Fast |

⭐ = Best Budget Choice (Unlimited, Very Fast)

### When to Use Which Model

**Composer 1:**
- ✅ Multi-file refactoring
- ✅ Component migrations
- ✅ TypeScript type updates
- ✅ Structured code changes

**Claude Sonnet 4.5:**
- ✅ Complex logic
- ✅ Architecture decisions
- ✅ Algorithm design
- ✅ Security-critical code

**Haiku 4.5:** ⭐ **Best Budget Choice**
- ✅ Quick iterations (Unlimited, Very Fast)
- ✅ Simple components
- ✅ Autocomplete
- ✅ Fast feedback
- ✅ **FREE/Unlimited** - No quota limits!

**GPT-5.1 Codex:**
- ✅ Code generation
- ✅ Boilerplate creation
- ✅ Quick code snippets

**Budget Models (Free/Unlimited):**
- ✅ **Haiku 4.5** - Best overall budget choice
- ✅ **GPT-5.1 Codex Fast** - Fast code generation
- ✅ **GPT-5.1 Codex Low Fast** - Ultra-fast iterations
- ✅ **GPT-5 Fast** - Quick general tasks
- ✅ **GPT-5 Low Fast** - Simplest tasks, fastest

---

## 📝 Customization Tips

### 1. Add Project-Specific Rules

```markdown
## Project-Specific Rules

### Our Conventions
- Use `@/` alias for imports
- Prefer named exports over default exports
- Use `const` assertions for type inference
```

### 2. Add Team Guidelines

```markdown
## Team Guidelines

### Code Review Checklist
- [ ] All tests pass
- [ ] TypeScript strict mode enabled
- [ ] No console.log in production code
- [ ] Error handling implemented
```

### 3. Add Domain-Specific Patterns

```markdown
## Domain-Specific Patterns

### Audio Processing
- Always use JUCE's AudioProcessor base class
- Pre-allocate buffers in prepareToPlay()
- No allocations in processBlock()
```

---

## 🔧 Advanced Usage

### Combine with Cursor Settings

Create `.vscode/settings.json` in your project:

```json
{
  "cursor.chat.defaultModel": "claude-sonnet-4.5",
  "cursor.composer.defaultModel": "composer-1",
  "cursor.general.enableCursorRules": true
}
```

### Use with Multiple Projects

If you work on multiple projects with different contexts:

1. Create separate workspace folders
2. Each workspace has its own `.cursorrules`
3. Cursor automatically uses the correct rules

---

## 📊 Benefits

### Token Savings
- **Without `.cursorrules`**: ~50-100 tokens per request (repeated instructions)
- **With `.cursorrules`**: 0 tokens (instructions already in context)
- **Savings**: 5,000-10,000 tokens per 100 requests

### Consistency
- Same rules applied automatically
- No need to remind AI about conventions
- Consistent code style across project

### Efficiency
- Faster AI responses (less context to process)
- Better code quality (rules always applied)
- Reduced manual corrections

---

## 🐛 Troubleshooting

### Rules Not Applied?

1. **Check file location**: `.cursorrules` must be in project root or parent folder
2. **Check file name**: Must be exactly `.cursorrules` (with dot)
3. **Restart Cursor**: Sometimes requires restart to pick up changes
4. **Check Cursor settings**: Ensure `cursor.general.enableCursorRules` is true

### Too Many Rules?

- Keep rules focused and relevant
- Remove outdated or unused rules
- Split into multiple files if needed (folder-specific)

### Model Not Following Rules?

- Rules are suggestions, not hard constraints
- Be explicit in your prompts
- Use `@model:` directive to specify model

---

## 📚 Additional Resources

- **Cursor Documentation**: https://cursor.sh/docs
- **Cursor Rules Guide**: See `CURSOR_EFFICIENCY_GUIDE.md` in your project
- **Model Reference**: See [Complete Model Reference](./Cursor%20AI%20Models%20-%20Complete%20Reference.md)
- **Changelog**: See [CHANGELOG.md](./CHANGELOG.md) for version history and updates

---

## 🤝 Contributing

Want to add more templates? Feel free to:
1. Create new template files
2. Improve existing templates
3. Share your context-specific rules

---

## 📄 License

These templates are provided as-is for use in your projects. Customize as needed for your specific requirements.

---

**Happy Coding! 🚀**

