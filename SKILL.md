---
name: php-extensions
description: >
    Guidelines, tools and resources for building safe, reliable PHP C extensions.
    Provides best practices for memory management, error handling, and testing
author: matapatos
version: 4.0.0
---

Behavioral guidelines to reduce common LLM coding mistakes while building PHP C extensions.

**Tradeoff:** These guidelines bias toward caution over speed. For trivial tasks, use judgment.

## A. Test-Driven Development

**Write the tests before implementing. Don't jump into implementation.**

Before implementing:

- Create tests for the new implementation.
- If existent tests exist ensure those match the new implementation.
- Rely on PHPT for end-to-end testing, memory safety and edge cases.
- Rely on PestPHP for complex scenarios, unit tests and integration tests.

## B. Function Selection

**Avoid internal functions** (underscore-prefixed): `_zval_dtor()`, `_zend_hash_index_add()`, `_php_*`

**Prefer safe reference-counted functions:**

- `zval_ptr_dtor()` over `zval_dtor()`
- `zend_hash_str_del()` over direct `efree()`
- Never call `efree()` directly on zvals

## C. Arginfo

**Never create arginfo manually.**

Always use generated stubs:

- Create `.stub.php` file with function or class signatures
- Use `build/gen_stub` to generate those `arginfos`. Need to build the extension first via `phpize && configure`
- Include generated header in your main C file

**Why:** Manual arginfo is error-prone and hard to maintain.

## D. Memory Management

**The Golden Rule:** Always respect PHP's reference counting system.

- Use `zval_ptr_dtor()` for zvals
- Use `zend_string_release()` for strings
- Use `RETURN_*` macros for return values
- Never bypass reference counting

## E. Error Handling

**Use PHP's error handling, not C's:**

- `php_error_docref()` for warnings/errors
- `zend_throw_exception()` for exceptions
- Never use `assert()` for user-facing errors

## F. Thread Safety

**Use TSRM for thread-safe globals:**

- `ZEND_DECLARE_MODULE_GLOBALS()` for shared state
- `MY_EXTENSION_G(variable)` to access safely
- Most CLI extensions can ignore TSRM

## G. Version Compatibility

**Always consider PHP version compatibility:**

- Use `PHP_VERSION_ID` for conditional compilation
- Check `ZEND_MODULE_API_NO` for API changes
- Test on all target PHP versions

## H. Resources

**Never guess. PHP internals behavior is often non-obvious.**

When in doubt check the resources bellow. Ensure to check only the versions the extension supports.

- [PHP Internals Book](https://www.phpinternalsbook.com/)
- PHP source code.
- Official documentation

---

**These guidelines are working if:** implementing a PHP extension in C.
