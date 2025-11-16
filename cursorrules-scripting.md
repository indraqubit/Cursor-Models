# .cursorrules - Scripting & Automation

**Version**: 1.0.0  
**Last Updated**: 2025-11-16  
**Context**: Scripting, Automation, DevOps, Utilities  
**Tech Stack**: Python, Shell (Bash/Zsh), Node.js scripts, Automation tools

---

## Project Context
This project contains scripts for automation, DevOps tasks, utilities, and tooling.
Focus areas: Script reliability, error handling, cross-platform compatibility, documentation.

---

## Model Recommendations

### For This Context, Prefer:
- **Haiku 4.5** - Quick scripts, simple automation, fast iterations
- **GPT-5.1 Codex** - Code generation, boilerplate creation
- **Claude Sonnet 4.5** - Complex automation logic, error handling
- **GPT-5.1 Codex Fast** - Quick script modifications

### Avoid:
- ❌ Heavy models for simple scripts (use Haiku)
- ❌ Slow models for quick iterations (use Fast variants)

### Budget-Friendly Options (Free/Unlimited):
- **Haiku 4.5** - Quick scripts, simple automation (Unlimited, Very Fast) ⭐ **Best Budget Choice**
- **GPT-5 Fast** - Quick script generation, simple edits (Unlimited, Fast)
- **GPT-5.1 Fast** - Latest GPT with fast responses (Unlimited, Fast)
- **GPT-5 Codex Fast** - Quick code generation (Unlimited, Fast)
- **GPT-5.1 Codex Fast** - Latest codex with speed (Unlimited, Fast)
- **GPT-5.1 Codex Low Fast** - Ultra-fast for rapid iterations (Unlimited, Very Fast)
- **GPT-5 Low Fast** - Simplest scripts, ultra-fast (Unlimited, Very Fast)

**When to Use Budget Models:**
- ✅ Simple automation scripts
- ✅ Quick iterations and experiments
- ✅ Basic shell scripts
- ✅ Simple Python utilities
- ✅ Development/testing scripts
- ✅ Documentation generation

**When NOT to Use Budget Models:**
- ❌ Complex automation logic
- ❌ Critical deployment scripts
- ❌ Production-critical automation
- ❌ Complex error handling

---

## Core Principles

### Scripting Best Practices
- **Error Handling** - Always handle errors gracefully
- **Exit Codes** - Use proper exit codes (0 = success, non-zero = error)
- **Logging** - Log important operations and errors
- **Idempotency** - Scripts should be safe to run multiple times
- **Documentation** - Document what scripts do and how to use them

### Cross-Platform Compatibility
- **Path Handling** - Use path.join() or path utilities
- **Line Endings** - Handle Windows vs Unix line endings
- **Shell Differences** - Test on multiple shells (bash, zsh, sh)
- **Environment Variables** - Use environment variables for configuration

---

## Code Style

### Naming Conventions
- **Scripts**: kebab-case (`deploy.sh`, `setup-env.py`, `build.mjs`)
- **Functions**: camelCase (JavaScript/Python) or snake_case (Python)
- **Variables**: camelCase (JavaScript) or snake_case (Python/Shell)
- **Constants**: UPPER_SNAKE_CASE (`MAX_RETRIES`, `DEFAULT_TIMEOUT`)

### File Organization
```
scripts/
├── build/              # Build scripts
│   ├── build.sh
│   └── deploy.sh
├── dev/                # Development scripts
│   ├── setup.sh
│   └── test.sh
├── utils/              # Utility scripts
│   ├── backup.sh
│   └── cleanup.py
└── automation/         # Automation scripts
    ├── sync.mjs
    └── monitor.py
```

---

## Shell Script Patterns

### Bash Script Template
```bash
#!/bin/bash
# @agent: cursor-ai | created: 2025-11-16T00:00:00+07:00 | modified: 2025-11-16T00:00:00+07:00

# Script: deploy.sh
# Purpose: Deploy application to production
# Usage: ./deploy.sh [environment]

set -euo pipefail  # Exit on error, undefined vars, pipe failures

# Configuration
ENVIRONMENT="${1:-production}"
SCRIPT_DIR="$(cd "$(dirname "${BASH_SOURCE[0]}")" && pwd)"
LOG_FILE="${SCRIPT_DIR}/logs/deploy-$(date +%Y%m%d-%H%M%S).log"

# Logging function
log() {
    echo "[$(date +'%Y-%m-%d %H:%M:%S')] $*" | tee -a "$LOG_FILE"
}

# Error handling
error_exit() {
    log "ERROR: $*"
    exit 1
}

# Main function
main() {
    log "Starting deployment to $ENVIRONMENT"
    
    # Pre-deployment checks
    if ! command -v node &> /dev/null; then
        error_exit "Node.js not found"
    fi
    
    # Deployment steps
    log "Building application..."
    npm run build || error_exit "Build failed"
    
    log "Deploying to $ENVIRONMENT..."
    # Deployment logic here
    
    log "Deployment completed successfully"
}

# Run main function
main "$@"
```

### Error Handling in Shell
```bash
# ✅ GOOD: Proper error handling
set -euo pipefail

# Check if command exists
if ! command -v git &> /dev/null; then
    echo "Error: git not found" >&2
    exit 1
fi

# Check if file exists
if [[ ! -f "$CONFIG_FILE" ]]; then
    echo "Error: Config file not found: $CONFIG_FILE" >&2
    exit 1
fi

# Trap errors
trap 'echo "Error at line $LINENO"' ERR
```

---

## Python Script Patterns

### Python Script Template
```python
#!/usr/bin/env python3
# @agent: cursor-ai | created: 2025-11-16T00:00:00+07:00 | modified: 2025-11-16T00:00:00+07:00

"""
Script: backup.py
Purpose: Backup database and files
Usage: python backup.py [--output-dir DIR]
"""

import argparse
import logging
import sys
from pathlib import Path
from typing import Optional

# Configure logging
logging.basicConfig(
    level=logging.INFO,
    format='%(asctime)s - %(levelname)s - %(message)s'
)
logger = logging.getLogger(__name__)


def backup_database(output_dir: Path) -> bool:
    """Backup database to output directory."""
    try:
        logger.info(f"Backing up database to {output_dir}")
        # Backup logic here
        return True
    except Exception as e:
        logger.error(f"Backup failed: {e}")
        return False


def main() -> int:
    """Main function."""
    parser = argparse.ArgumentParser(description='Backup database and files')
    parser.add_argument(
        '--output-dir',
        type=Path,
        default=Path('./backups'),
        help='Output directory for backups'
    )
    
    args = parser.parse_args()
    
    # Validate output directory
    if not args.output_dir.exists():
        args.output_dir.mkdir(parents=True, exist_ok=True)
    
    # Run backup
    if not backup_database(args.output_dir):
        return 1
    
    logger.info("Backup completed successfully")
    return 0


if __name__ == '__main__':
    sys.exit(main())
```

### Error Handling in Python
```python
# ✅ GOOD: Proper error handling
import sys
from pathlib import Path

def process_file(file_path: Path) -> bool:
    try:
        if not file_path.exists():
            raise FileNotFoundError(f"File not found: {file_path}")
        
        # Process file
        with open(file_path, 'r') as f:
            content = f.read()
        
        return True
    except FileNotFoundError as e:
        print(f"Error: {e}", file=sys.stderr)
        return False
    except Exception as e:
        print(f"Unexpected error: {e}", file=sys.stderr)
        return False
```

---

## Node.js Script Patterns

### Node.js Script Template
```javascript
#!/usr/bin/env node
// @agent: cursor-ai | created: 2025-11-16T00:00:00+07:00 | modified: 2025-11-16T00:00:00+07:00

/**
 * Script: sync.mjs
 * Purpose: Sync files between directories
 * Usage: node sync.mjs [source] [destination]
 */

import { readFile, writeFile } from 'fs/promises';
import { join } from 'path';
import { fileURLToPath } from 'url';

const __filename = fileURLToPath(import.meta.url);
const __dirname = join(__filename, '..');

// Error handling
function handleError(error, context) {
  console.error(`Error in ${context}:`, error.message);
  process.exit(1);
}

// Main function
async function main() {
  try {
    const [source, destination] = process.argv.slice(2);
    
    if (!source || !destination) {
      console.error('Usage: node sync.mjs <source> <destination>');
      process.exit(1);
    }
    
    console.log(`Syncing ${source} to ${destination}`);
    // Sync logic here
    
    console.log('Sync completed successfully');
  } catch (error) {
    handleError(error, 'main');
  }
}

main();
```

---

## Common Script Patterns

### File Operations
```bash
# ✅ GOOD: Safe file operations
BACKUP_DIR="./backups"
mkdir -p "$BACKUP_DIR"

# Create backup with timestamp
BACKUP_FILE="${BACKUP_DIR}/backup-$(date +%Y%m%d-%H%M%S).tar.gz"
tar -czf "$BACKUP_FILE" "$SOURCE_DIR" || {
    echo "Backup failed" >&2
    exit 1
}
```

### Environment Variables
```bash
# ✅ GOOD: Environment variable handling
ENVIRONMENT="${ENVIRONMENT:-development}"
API_KEY="${API_KEY:?API_KEY environment variable is required}"

# Use default if not set
TIMEOUT="${TIMEOUT:-30}"
```

### Conditional Execution
```bash
# ✅ GOOD: Conditional execution
if [[ "$DRY_RUN" == "true" ]]; then
    echo "DRY RUN: Would execute: $COMMAND"
else
    eval "$COMMAND"
fi
```

---

## Logging & Output

### Structured Logging
```python
# ✅ GOOD: Structured logging in Python
import logging
import json

logging.basicConfig(
    level=logging.INFO,
    format='%(asctime)s - %(name)s - %(levelname)s - %(message)s'
)

logger = logging.getLogger(__name__)

# Log with context
logger.info("Processing file", extra={
    "file": file_path,
    "size": file_size,
    "action": "process"
})
```

### Progress Indicators
```bash
# ✅ GOOD: Progress indicator
total=100
current=0

while [ $current -lt $total ]; do
    current=$((current + 1))
    percent=$((current * 100 / total))
    printf "\rProgress: [%-50s] %d%%" "$(printf '#%.0s' $(seq 1 $((percent/2))))" "$percent"
    sleep 0.1
done
echo
```

---

## Testing Scripts

### Script Testing
```bash
# ✅ GOOD: Test script functionality
#!/bin/bash

test_backup() {
    echo "Testing backup function..."
    # Test logic
    if backup_database "./test-backups"; then
        echo "✓ Backup test passed"
        return 0
    else
        echo "✗ Backup test failed"
        return 1
    fi
}

# Run tests
test_backup
```

---

## Documentation

### Script Documentation
```bash
#!/bin/bash
# @agent: cursor-ai | created: 2025-11-16T00:00:00+07:00 | modified: 2025-11-16T00:00:00+07:00

# Script: deploy.sh
# 
# Purpose: Deploy application to specified environment
# 
# Usage:
#   ./deploy.sh [environment]
#   ./deploy.sh production
#   ./deploy.sh staging
#
# Environment Variables:
#   - DEPLOY_KEY: Required. Deployment key for authentication
#   - ENVIRONMENT: Optional. Default: production
#
# Exit Codes:
#   0 - Success
#   1 - Error
#
# Examples:
#   DEPLOY_KEY=abc123 ./deploy.sh production
#   ./deploy.sh staging
```

---

## Anti-Patterns to Avoid

❌ **No Error Handling**
```bash
# BAD
rm -rf "$DIR"  # No check if DIR exists or is safe to delete!
```

❌ **Hardcoded Paths**
```bash
# BAD
cd /home/user/project  # Hardcoded path!
```

❌ **No Input Validation**
```bash
# BAD
rm -rf "$1"  # No validation of input!
```

❌ **Silent Failures**
```bash
# BAD
command > /dev/null 2>&1  # Hides all errors!
```

---

## Model Selection Guide

| Task Type | Recommended Model | Budget Alternative | Reason |
|-----------|-------------------|-------------------|--------|
| **Simple Scripts** | Haiku 4.5 | GPT-5.1 Codex Low Fast | Fast, good enough |
| **Quick Iterations** | Haiku 4.5 | GPT-5 Low Fast | Very fast |
| **Complex Automation** | Claude Sonnet 4.5 | Haiku 4.5 (simple) | Best reasoning |
| **Code Generation** | GPT-5.1 Codex | GPT-5.1 Codex Fast | Specialized |
| **Error Handling** | Claude Sonnet 4.5 | Haiku 4.5 (simple) | Best for logic |
| **Cross-Platform** | Claude Sonnet 4.5 | GPT-5.1 Fast | Better understanding |
| **Basic Shell Scripts** | Haiku 4.5 | GPT-5 Low Fast | Best budget choice |
| **Quick Utilities** | Haiku 4.5 | GPT-5.1 Codex Low Fast | Simple automation |

---

**Remember**: Scripts should be reliable, well-documented, and safe to run. Always handle errors gracefully and provide clear feedback.

