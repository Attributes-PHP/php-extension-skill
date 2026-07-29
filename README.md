# Attributes PHP Extension Skill

```
        ___  _ _  ___            _                 _                 _    _  _  _
 _|_|_ | . \| | || . \  ___ __ _| |_ ___ ._ _  ___<_> ___ ._ _   ___| |__<_>| || |
 _|_|_ |  _/|   ||  _/ / ._>\ \/| | / ._>| ' |<_-<| |/ . \| ' | <_-<| / /| || || |
  | |  |_|  |_|_||_|   \___./\_\|_| \___.|_|_|/__/|_|\___/|_|_| /__/|_\_\|_||_||_|
```

Guidelines, tools and resources for building safe, reliable PHP C extensions. Provides best practices for memory management, error handling, and testing.

---

## Why This Skill?

Writing PHP C extensions is powerful but unforgiving. One wrong reference count and your PHP process crashes. This skill gives you the guardrails to build extensions that work reliably across PHP versions.

## The 8 Rules of Safe PHP Extensions

These are the core principles that prevent segfaults, memory leaks, and version incompatibilities:

| # | Rule | Why It Matters |
|---|------|----------------|
| A | **Test-Driven Development** | Write tests before implementing; rely on PHPT and PestPHP |
| B | **Avoid `_` functions** | Internal APIs change without notice |
| C | **Always use generated arginfo** | Manual arginfo is error-prone and hard to maintain |
| D | **Respect reference counting** | The Golden Rule of PHP memory management |
| E | **Use PHP error handling** | `php_error_docref()`, `zend_throw_exception()`, not `assert()` |
| F | **Use TSRM for shared state** | Thread-safe globals when needed |
| G | **Check version compatibility** | PHP 7, 8.0, 8.1+ all behave differently |
| H | **Consult reliable resources** | PHP internals behavior is non-obvious |

See [SKILL.md](SKILL.md) for the full guidelines.

---

## System Requirements

- **Cross-Platform**: Works on Linux, macOS, Windows (Git Bash/WSL/PowerShell)
- **PHP Versions**: Supports PHP 8.5 and all previous versions
- **Dependencies**: Git, standard C development tools

---

Attributes PHP extension skill was created by **[André Gil](https://www.linkedin.com/in/andre-gil/)** and is open-sourced software licensed under the **[MIT license](https://opensource.org/licenses/MIT)**.