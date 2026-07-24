# php-extension-skill

A Vibe skill for PHP extension development and PHP internals lookup.

## Overview

This skill enables AI agents to answer questions about PHP extensions, internals, and the Zend Engine by automatically downloading and maintaining the PHP source code repository (`php-src`).

## Features

- **Source Code Lookup**: Automatically clones and maintains `php-src` in a temporary folder
- **Cross-Platform**: Works on Windows (Git Bash/WSL, PowerShell), Linux, and macOS
- **PHP Version Support**: Supports PHP 8.5 and all previous versions
- **Comprehensive Documentation**: Detailed information about PHP internals, extension API, Zend Engine, and more

## Use Cases

- Look up PHP extension API functions (`zend_*`, `php_*`, `ZEND_*` macros)
- Understand extension implementation details
- Explore PHP internals and Zend Engine behavior
- Research extension configuration and php.ini directives
- Investigate PHP function implementations
- Check version-specific extension changes

## Quick Start

When loaded, this skill:
1. Sets up `${SCRATCHPAD}/.php-extensions/php-src/` with the PHP source code
2. Provides search and navigation commands for the codebase
3. Offers examples and patterns for common PHP internals questions

## Documentation

See [SKILL.md](SKILL.md) for complete documentation, including:
- Repository setup and maintenance commands
- PHP source code structure
- Common patterns and APIs
- Debugging and development tips
- Troubleshooting guide

---

**Skill Metadata:**
- Name: `php-extensions`
- Version: 2.1.0
- Author: matapatos
