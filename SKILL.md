PHP C Extension Specific Guidelines. Merge with project-specific instructions as needed.

**Tradeoff:** These guidelines bias toward safety over convenience. For trivial tasks, use judgment.

## 1. Information Lookup Priority

**Never guess — PHP internals behavior is often non-obvious.**

- **First:** [PHP Internals Book](https://www.phpinternalsbook.com/)
- **Second:** PHP source code examples
- **Third:** Official documentation

## 2. Function Selection

**Avoid internal functions** (underscore-prefixed): `_zval_dtor()`, `_zend_hash_index_add()`, `_php_*`

**Prefer safe reference-counted functions:**
- ✅ `zval_ptr_dtor()` over `zval_dtor()`
- ✅ `zend_hash_str_del()` over direct `efree()`
- ❌ Never call `efree()` directly on zvals

## 3. Arginfo

**Never create arginfo manually.** Always use generated stubs:
- Create `.stub.php` file with function or class signatures
- Use `build/gen_stub` to generate those `arginfos`
- Include generated header in your main C file

**Why:** Manual arginfo is error-prone and hard to maintain.

## 4. Testing

**Always test with PHPT for core extension functionality.**

- PHPT: C-level extension, memory safety, edge cases
- PestPHP: Complex scenarios, integration, faster iteration

**Workflow:**
1. PHPT tests for every function
2. Edge cases and error conditions
3. Valgrind to verify no memory leaks

## 5. Memory Management

**The Golden Rule:** Always respect PHP's reference counting system.

- Use `zval_ptr_dtor()` for zvals
- Use `zend_string_release()` for strings
- Use `RETURN_*` macros for return values
- Never bypass reference counting

## 6. Error Handling

**Use PHP's error handling, not C's:**
- `php_error_docref()` for warnings/errors
- `zend_throw_exception()` for exceptions
- ❌ Never use `assert()` for user-facing errors

## 7. Version Compatibility

**Always consider PHP version compatibility:**
- Use `PHP_VERSION_ID` for conditional compilation
- Check `ZEND_MODULE_API_NO` for API changes
- Test on all target PHP versions

## 8. Thread Safety

**Use TSRM for thread-safe globals:**
- `ZEND_DECLARE_MODULE_GLOBALS()` for shared state
- `MY_EXTENSION_G(variable)` to access safely
- Most CLI extensions can ignore TSRM

---

**These guidelines are working if:** Fewer segfaults, memory leaks caught early, arginfo never manually written, and every feature has comprehensive tests.
