# PHP C Extension Skill

Build safe, reliable PHP extensions with confidence.

---

## Why This Skill?

Writing PHP C extensions is powerful but unforgiving. One wrong reference count and your PHP process crashes. This skill gives you the guardrails to build extensions that work reliably across PHP versions.

```
        ___  _ _  ___            _                 _                 _    _  _  _
 _|_|_ | . \| | || . \  ___ __ _| |_ ___ ._ _  ___<_> ___ ._ _   ___| |__<_>| || |
 _|_|_ |  _/|   ||  _/ / ._>\ \/| | / ._>| ' |<_-<| |/ . \| ' | <_-<| / /| || || |
  | |  |_|  |_|_||_|   \___./\_\|_| \___.|_|_|/__/|_|\___/|_|_| /__/|_\_\|_||_||_|
```

## The 8 Rules of Safe PHP Extensions

These are the core principles that prevent segfaults, memory leaks, and version incompatibilities:

| # | Rule | Why It Matters |
|---|------|----------------|
| 1 | **Consult the Internals Book first** | PHP internals behavior is non-obvious |
| 2 | **Avoid `_` functions** | Internal APIs change without notice |
| 3 | **Always use generated arginfo** | Manual arginfo is error-prone |
| 4 | **Test with PHPT** | C-level functionality needs C-level tests |
| 5 | **Respect reference counting** | The Golden Rule of PHP memory management |
| 6 | **Use PHP error handling** | `php_error_docref()`, not `assert()` |
| 7 | **Check version compatibility** | PHP 7, 8.0, 8.1+ all behave differently |
| 8 | **Use TSRM for shared state** | Thread-safe globals when needed |

See [SKILL.md](SKILL.md) for the full guidelines.

---

## What This Skill Does

When loaded, this skill:

1. **Sets up PHP source** – Clones `php-src` to `${SCRATCHPAD}/.php-extensions/php-src/`
2. **Enables smart lookup** – Search and navigate the PHP internals codebase
3. **Provides patterns** – Examples and solutions for common extension challenges

---

## Practical Use Cases

- **"How do I properly return a zval?"** → `RETURN_ZVAL(&zv, 1, 0)`
- **"What's the safe way to destroy a zval?"** → `zval_ptr_dtor(&zv)`
- **"How do I handle PHP version differences?"** → Use `PHP_VERSION_ID` checks
- **"Where do I find the Zend API docs?"** → [PHP Internals Book](https://www.phpinternalsbook.com/)

---

## Quick Reference

| Need | Use | Avoid |
|------|-----|--------|
| Destroy zval | `zval_ptr_dtor()` | `zval_dtor()`, `efree()` |
| Add to hashtable | `zend_hash_str_add_new()` | direct insertion |
| Return string | `RETURN_STRING()`, `RETURN_NEW_STR()` | manual allocation |
| Error handling | `php_error_docref()`, `zend_throw_exception()` | `assert()` |

---

## System Requirements

- **Cross-Platform**: Works on Linux, macOS, Windows (Git Bash/WSL/PowerShell)
- **PHP Versions**: Supports PHP 8.5 and all previous versions
- **Dependencies**: Git, standard C development tools

---

**Created by [Andre Gil](https://www.linkedin.com/in/andre-gil/) | MIT License**