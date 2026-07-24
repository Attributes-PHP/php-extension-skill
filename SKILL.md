---
name: php-extensions
description: >
    Look up PHP extension-related questions directly from the PHP source code repository.
    Automatically downloads and maintains the php-src repository in a temporary .php-extensions
    folder for cross-platform source code lookups. Supports PHP 8.5 and all previous versions.
author: matapatos
version: 2.1.0
---

# PHP Extensions Source Lookup

**Purpose:** Look up PHP extension-related questions directly from the PHP source code. This skill automatically downloads and maintains the `php-src` repository in a temporary `.php-extensions` folder, enabling cross-platform (Windows, Linux, macOS) source code lookups for PHP internals, Zend Engine, and extension implementations.

---

## Repository Setup

This skill uses a local copy of the PHP source code repository (`php-src`) stored in a temporary folder for all lookups. The repository is automatically downloaded and kept up-to-date.

### Repository Location
- **Path:** `${SCRATCHPAD}/.php-extensions/php-src/`
- **Scratchpad:** The session-local temporary directory provided to the agent
- **Structure:** `.php-extensions/php-src/` contains the full php-src repository

### Initialization Commands

Run these commands to set up the repository. These are cross-platform and work on Windows (Git Bash/WSL), Linux, and macOS:

```bash
# Navigate to scratchpad directory
cd "${SCRATCHPAD}"

# Create the .php-extensions directory
mkdir -p .php-extensions

# Clone php-src repository (if not already present)
if [ ! -d ".php-extensions/php-src" ]; then
    git clone https://github.com/php/php-src.git .php-extensions/php-src
fi

# Navigate to the repository
cd .php-extensions/php-src

# Update to latest master
git fetch origin
git checkout master
git pull origin master
```

**Windows (PowerShell alternative):**
```powershell
# Navigate to scratchpad
cd "${SCRATCHPAD}"

# Create directory
New-Item -ItemType Directory -Force -Path .php-extensions

# Clone repository
if (-not (Test-Path .php-extensions/php-src)) {
    git clone https://github.com/php/php-src.git .php-extensions/php-src
}

# Update repository
cd .php-extensions/php-src
git fetch origin
git checkout master
git pull origin master
```

### Repository Maintenance

**Update to latest version:**
```bash
cd "${SCRATCHPAD}/.php-extensions/php-src"
git pull origin master
```

**Check current version:**
```bash
cd "${SCRATCHPAD}/.php-extensions/php-src"
git log -1 --pretty=format:"%H %s"
git describe --tags
```

**Clean and reset:**
```bash
cd "${SCRATCHPAD}"
rm -rf .php-extensions
mkdir -p .php-extensions
git clone https://github.com/php/php-src.git .php-extensions/php-src
```

**Checkout specific PHP version:**
```bash
cd "${SCRATCHPAD}/.php-extensions/php-src"
# For PHP 8.5 (latest development)
git checkout PHP-8.5

# For PHP 8.4
git checkout PHP-8.4

# For PHP 8.3
git checkout PHP-8.3

# For PHP 8.2
git checkout PHP-8.2

# For latest stable (master branch)
git checkout master
```

---

## When to Use This Skill

Use this skill when the user asks about:
- PHP extension API functions (`zend_*`, `php_*`, `ZEND_*` macros)
- Extension implementation details (e.g., "How does the JSON extension work internally?")
- PHP internals and Zend Engine behavior
- Extension configuration (php.ini directives)
- Extension build system and SAPI modules
- PHP function implementations (e.g., "How is `array_map` implemented?")
- INI settings and their effects
- PHP version-specific extension changes

**Do NOT use** for:
- General PHP usage questions (use standard PHP documentation)
- Questions about userland PHP code
- Framework-specific questions (Laravel, Symfony, etc.)
- Questions not related to PHP internals or extensions

---

## PHP Source Code Structure

The repository is located at: `${SCRATCHPAD}/.php-extensions/php-src/`

````
.php-extensions/php-src/
├── ext/                    # All bundled PHP extensions
│   ├── standard/           # Core functions (array, string, etc.)
│   ├── json/               # JSON extension
│   ├── pdo/                # PDO and its drivers
│   ├── spl/                # Standard PHP Library
│   └── ...                 # Other bundled extensions
│
├── Zend/                   # Zend Engine source
│   ├── zend.h              # Main Zend Engine header
│   ├── zend_API.h          # Public Zend API
│   ├── zend_alloc.c        # Memory management
│   ├── zend_compile.c      # Compilation
│   ├── zend_execute.c      # Execution engine
│   ├── zend_object_handlers.c
│   ├── zend_operators.c
│   └── ...
│
├── main/                   # Main PHP core
│   ├── php.h               # Main PHP header
│   ├── php_globals.h       # Global variables
│   ├── php_main.c          # Main entry point
│   └── ...
│
├── TSRM/                   # Thread Safe Resource Manager
├── sapi/                   # SAPI modules (CLI, FPM, Apache, etc.)
├── include/                # Public headers
│   ├── php/                # PHP headers
│   │   └── ext/            # Extension headers
│   └── Zend/               # Zend headers
│
└── configure.ac / CMakeLists.txt  # Build system
````

---

## Platform-Specific Notes

### Linux / macOS
- All commands work natively in bash, zsh, or sh
- Use forward slashes for paths
- Git is typically pre-installed or available via package manager

### Windows
- Use Git Bash, WSL, or PowerShell for best results
- In PowerShell, use the commands shown in the "Windows (PowerShell alternative)" section
- In Git Bash/WSL, use the standard bash commands
- For file paths, you can use either forward or backward slashes

### Cross-Platform Command Equivalence

| Action | Linux/macOS | Windows (PowerShell) | Windows (Git Bash) |
|--------|-------------|---------------------|-------------------|
| Change directory | `cd /path/to/dir` | `cd "C:\path\to\dir"` | `cd /c/path/to/dir` |
| List files | `ls` | `Get-ChildItem` or `dir` | `ls` |
| Make directory | `mkdir -p dir` | `New-Item -ItemType Directory dir` | `mkdir -p dir` |
| Check file exists | `[ -d "dir" ]` | `Test-Path dir` | `[ -d "dir" ]` |
| Clone repo | `git clone url dir` | `git clone url dir` | `git clone url dir` |

---

## Key Directories and Files

All paths are relative to `${SCRATCHPAD}/.php-extensions/php-src/`

### Extension Structure (example: `ext/json/`)
```
ext/json/
├── config.m4               # Autoconf build configuration
├── config.w32              # Windows build configuration
├── php_json.h              # Public header (exposed to PHP)
├── json.c                  # Main implementation
├── php_json_int.h          # Internal header
├── json_stub.c             # Stub for static compilation
└── tests/                  # Extension tests
```

### Zend Engine Key Files
- `Zend/zend.h` - Main header with core structures
- `Zend/zend_API.h` - Public Zend API for extensions
- `Zend/zend_alloc.h` - Memory allocation
- `Zend/zend_compile.h` - Compilation API
- `Zend/zend_execute.h` - Execution API
- `Zend/zend_operators.h` - Operator implementations
- `Zend/zend_object_handlers.h` - Object handlers
- `Zend/zend_types.h` - Type definitions

### Main PHP Core Files
- `main/php.h` - Global PHP definitions
- `main/php_globals.h` - Global state
- `main/php_main.c` - Main entry point
- `main/php_ini.c` - INI configuration handling
- `main/php_variables.c` - Variable handling

---

## Common PHP Internals Patterns

### Function Implementation
PHP functions are typically implemented as:

```c
PHP_FUNCTION(function_name)
{
    zval *arg1, *arg2;
    ZEND_PARSE_PARAMETERS_START(2, 2)
        Z_PARAM_ZVAL(arg1)
        Z_PARAM_ZVAL(arg2)
    ZEND_PARSE_PARAMETERS_END();
    
    // Implementation
    RETURN_TRUE;
}
```

### Function Registration
Functions are registered in module startup:

```c
static const zend_function_entry json_functions[] = {
    PHP_FE(json_encode,    arginfo_json_encode)
    PHP_FE(json_decode,    arginfo_json_decode)
    PHP_FE_END
};

zend_module_entry json_module_entry = {
    STANDARD_MODULE_HEADER,
    "json",
    json_functions,
    NULL, /* Module startup */
    NULL, /* Module shutdown */
    NULL, /* Request startup */
    NULL, /* Request shutdown */
    NULL, /* Module info */
    PHP_JSON_VERSION,
    STANDARD_MODULE_PROPERTIES
};
```

### INI Configuration

```c
PHP_INI_BEGIN()
    PHP_INI_ENTRY("json.default_depth", "512", PHP_INI_ALL, NULL)
PHP_INI_END()
```

### Class Implementation

```c
zend_class_entry *php_json_exception_ce;

PHP_MINIT_FUNCTION(json)
{
    zend_class_entry ce;
    INIT_CLASS_ENTRY(ce, "JsonException", NULL);
    php_json_exception_ce = zend_register_internal_class_exception(&ce, zend_exception_get_default());
    
    zend_declare_property_null(php_json_exception_ce, "message", sizeof("message")-1, ZEND_ACC_PUBLIC);
    // ...
    return SUCCESS;
}
```

---

## Important Macros and Structures

### ZVAL and zval
```c
typedef struct _zval_struct {
    zend_value        value;        // The actual value
    zend_uint         type;         // Type (IS_NULL, IS_LONG, etc.)
    zend_uint         type_flags;   // Additional type info
    zend_uint         refcount;     // Reference count
} zval;

// Common types:
#define IS_NULL       0
#define IS_FALSE      2
#define IS_TRUE       3
#define IS_LONG       4
#define IS_DOUBLE     5
#define IS_STRING     6
#define IS_ARRAY      7
#define IS_OBJECT     8
#define IS_RESOURCE   10
#define IS_REFERENCE  11
```

### ZEND_PARSE_PARAMETERS
Used to parse function arguments:

```c
ZEND_PARSE_PARAMETERS_START(min_num_args, max_num_args)
    Z_PARAM_LONG(a)           // long parameter
    Z_PARAM_STR(s)            // string parameter
    Z_PARAM_ZVAL(z)           // zval parameter
    Z_PARAM_ARRAY(a)          // array parameter
    Z_PARAM_OBJECT(o)         // object parameter
    Z_PARAM_RESOURCE(r)       // resource parameter
    Z_PARAM_BOOL(b)           // boolean parameter
    Z_PARAM_PATH(p, p_len)    // path parameter
    Z_PARAM_OPTIONAL          // optional parameter follows
ZEND_PARSE_PARAMETERS_END();
```

### Return Macros
```c
RETURN_TRUE;        // return true
RETURN_FALSE;       // return false
RETURN_LONG(l);     // return long
RETURN_STR(s);      // return string (duplicates)
RETURN_STRING(s);   // return string literal
RETURN_ZVAL(zv, copy, dtor);
RETVAL_TRUE;       // set return value to true
RETVAL_FALSE;      // set return value to false
RETVAL_LONG(l);    // set return value to long
RETVAL_STRING(s);  // set return value to string
```

### Zend Engine Structures
```c
// zend_function_entry
typedef struct _zend_function_entry {
    const char *fname;
    void (*handler)(INTERNAL_FUNCTION_PARAMETERS);
    struct _zend_internal_arg_info *arg_info;
    uint32_t num_args;
    uint32_t flags;
} zend_function_entry;

// zend_module_entry
typedef struct _zend_module_entry {
    unsigned short size;
    unsigned int zend_api;
    unsigned char zend_debug;
    unsigned char zts;
    const struct _zend_ini_entry *ini_entry;
    const struct _zend_module_dep *deps;
    const char *name;
    const struct _zend_function_entry *functions;
    int (*module_startup_func)(INTERNAL_MODULE_PARAMETERS);
    int (*module_shutdown_func)(INTERNAL_MODULE_PARAMETERS);
    int (*request_startup_func)(INTERNAL_MODULE_PARAMETERS);
    int (*request_shutdown_func)(INTERNAL_MODULE_PARAMETERS);
    void (*info_func)(zend_module_entry *zend_module);
    const char *version;
    // ...
} zend_module_entry;
```

---

## Workflow for Answering PHP Extension Questions

### 1. Initialize Repository (if not already done)

First, ensure the php-src repository is available:

```bash
# Set base directory
PHP_SRC_DIR="${SCRATCHPAD}/.php-extensions/php-src"

# Check and initialize if needed
if [ ! -d "$PHP_SRC_DIR" ]; then
    mkdir -p "${SCRATCHPAD}/.php-extensions"
    git clone https://github.com/php/php-src.git "$PHP_SRC_DIR"
fi

# Ensure we're on master
cd "$PHP_SRC_DIR"
git checkout master 2>/dev/null
git pull origin master 2>/dev/null
```

### 2. Identify the Component
Determine if the question is about:
- A specific **extension** (e.g., json, pdo, mbstring)
- The **Zend Engine** (execution, compilation, memory management)
- **SAPI modules** (CLI, FPM, Apache)
- **Core PHP** (main/, TSRM/)

### 3. Locate the Source Files

**For extensions:**
- Look in `$PHP_SRC_DIR/ext/<extension_name>/`
- Check `$PHP_SRC_DIR/ext/<extension_name>/php_<extension_name>.h` for public API
- Check `$PHP_SRC_DIR/ext/<extension_name>/<extension_name>.c` for implementation

**For Zend Engine:**
- Look in `$PHP_SRC_DIR/Zend/` directory
- Use `$PHP_SRC_DIR/Zend/zend_API.h` for public API
- Check specific files based on the subsystem

**For SAPI:**
- Look in `$PHP_SRC_DIR/sapi/<sapi_name>/` (e.g., `sapi/cli/`, `sapi/fpm/`)

**For core functions:**
- Look in `$PHP_SRC_DIR/ext/standard/` for most built-in functions
- Use grep to find function definitions

### 4. Search Strategies

Use the bash tool with the following commands from `$PHP_SRC_DIR`:

**Find function implementation:**
```bash
# From PHP_SRC_DIR
grep -r "PHP_FUNCTION(function_name)" ext/
grep -r "zend_function_entry.*function_name" ext/

# Find internal functions (zif_ prefix)
grep -r "zif_function_name" ext/

# Find class implementations
grep -r "zend_class_entry.*ClassName" ext/
```

**Find macro definitions:**
```bash
grep -r "#define MACRO_NAME" Zend/ include/ ext/
```

**Find structure definitions:**
```bash
grep -r "typedef struct.*_name" Zend/ include/ ext/
```

**Find INI entries:**
```bash
grep -r "PHP_INI_ENTRY.*setting_name" ext/
```

### 5. Read and Analyze

When reading the source using the read_file tool:
1. Use absolute paths: `${SCRATCHPAD}/.php-extensions/php-src/path/to/file.c`
2. Start with the header file to understand the public API
3. Read the implementation file
4. Look for `PHP_MINIT_FUNCTION`, `PHP_MSHUTDOWN_FUNCTION`, etc.
5. Check the `zend_module_entry` structure
6. Review the function entries in the module

### 6. Cross-Reference

- Check `$PHP_SRC_DIR/include/php/ext/` for extension headers
- Check `$PHP_SRC_DIR/include/Zend/` for Zend headers
- Look at tests in `$PHP_SRC_DIR/ext/<extension>/tests/`
- Check NEWS file for version changes

---

## Example Lookups

### Example 1: How does `json_encode` work internally?

1. **Initialize repository:**
   ```bash
   PHP_SRC_DIR="${SCRATCHPAD}/.php-extensions/php-src"
   if [ ! -d "$PHP_SRC_DIR" ]; then
       mkdir -p "${SCRATCHPAD}/.php-extensions"
       git clone https://github.com/php/php-src.git "$PHP_SRC_DIR"
   fi
   ```

2. **Find the file:**
   ```bash
   cd "$PHP_SRC_DIR"
   grep -r "PHP_FUNCTION(json_encode)" ext/json/
   ```
   -> `ext/json/json.c`

3. **Read the function:**
   - Use `read_file` with path: `${SCRATCHPAD}/.php-extensions/php-src/ext/json/json.c`
   - Look at `PHP_FUNCTION(json_encode)`
   - Check parameters with `ZEND_PARSE_PARAMETERS`
   - Follow the implementation logic

4. **Understand dependencies:**
   - Check if it calls any Zend Engine functions
   - Look at the JSON encoder implementation

### Example 2: What are the INI settings for the OPcache extension?

1. **Find the extension:**
   ```bash
   cd "$PHP_SRC_DIR"
   ls ext/opcache/
   ```

2. **Find INI entries:**
   ```bash
   grep -r "PHP_INI_ENTRY" ext/opcache/
   ```

3. **Read the configuration:**
   - Read `${SCRATCHPAD}/.php-extensions/php-src/ext/opcache/php_opcache.h`
   - Check `PHP_MINIT_FUNCTION(opcache)` in opcache.c
   - Look at the `php_opcache_globals` structure

### Example 3: How are PHP arrays implemented?

1. **This is a Zend Engine question**
2. **Find relevant files:**
   ```bash
   cd "$PHP_SRC_DIR"
   grep -r "zend_array" Zend/
   grep -r "HashTable" Zend/
   ```
3. **Key files:**
   - `${SCRATCHPAD}/.php-extensions/php-src/Zend/zend_hash.h` - HashTable implementation
   - `${SCRATCHPAD}/.php-extensions/php-src/Zend/zend_array.h` - Array-specific code
   - `${SCRATCHPAD}/.php-extensions/php-src/Zend/zend_types.h` - Type definitions

---

## Important Zend Engine APIs

### Memory Management
```c
// Allocation
void *emalloc(size_t size);
void *ecalloc(size_t nmemb, size_t size);
void *erealloc(void *ptr, size_t size);
char *estrdup(const char *s);
char *estrndup(const char *s, size_t length);

// Free
void efree(void *ptr);
void efree_size(void *ptr, size_t size);

// Smart strings
zend_string *zend_string_init(const char *str, size_t len, int persistent);
zend_string *zend_string_alloc(size_t len, int persistent);
void zend_string_free(zend_string *s);
void zend_string_release(zend_string *s);
```

### HashTable API
```c
// Creation
HashTable *zend_new_array(uint32_t size);
HashTable *zend_new_hash_table(uint32_t size);

// Insertion
zend_hash_index_add(HashTable *ht, zend_ulong h, zval *pData);
zend_hash_add(HashTable *ht, const char *key, size_t key_len, zval *pData, int copy);
zend_hash_str_add(HashTable *ht, const char *key, size_t key_len, zval *pData);
zend_hash_index_add_new(HashTable *ht, zend_ulong h, zval *pData);
zend_hash_add_new(HashTable *ht, const char *key, size_t key_len, zval *pData);

// Lookup
zval *zend_hash_index_find(const HashTable *ht, zend_ulong h);
zval *zend_hash_find(const HashTable *ht, const char *key, size_t key_len);
zval *zend_hash_str_find(const HashTable *ht, const char *key, size_t key_len);

// Iteration
ZEND_HASH_FOREACH_KEY_VAL(ht, h, key, val) { /* ... */ } ZEND_HASH_FOREACH_END();
ZEND_HASH_FOREACH_VAL(ht, val) { /* ... */ } ZEND_HASH_FOREACH_END();

// Deletion
int zend_hash_index_del(HashTable *ht, zend_ulong h);
int zend_hash_del(HashTable *ht, const char *key, size_t key_len);
int zend_hash_str_del(HashTable *ht, const char *key, size_t key_len);

// Destruction
void zend_hash_destroy(HashTable *ht);
void zend_hash_clean(HashTable *ht);
```

### Object Handling
```c
// Create object
zend_object *zend_objects_new(zend_class_entry *ce);

// Call method
zval zmethod;
ZVAL_STRING(&zmethod, "methodName");
zend_call_method(object, ce, &zmethod, return_value_ptr, param_count, params, NULL);

// Property access
zval *zend_read_property(zend_class_entry *ce, zval *object, const char *name, size_t name_length, int silent, zval *rv);
void zend_update_property(zend_class_entry *ce, zval *object, const char *name, size_t name_length, zval *value);
void zend_write_property(zend_class_entry *ce, zval *object, const char *name, size_t name_length, zval *value);

// Class registration
zend_class_entry *zend_register_internal_class(zend_class_entry *class_entry);
zend_class_entry *zend_register_internal_interface(zend_class_entry *class_entry);
```

### Error Handling
```c
// Throw exception
zend_throw_exception(zend_class_entry *exception_ce, const char *message, int code);
zend_throw_exception_ex(zend_class_entry *exception_ce, long code, const char *format, ...);

// Error reporting
void php_error_docref(const char *docref, int type, const char *format, ...);

// Error level types:
#define E_ERROR         1
#define E_WARNING       2
#define E_PARSE         4
#define E_NOTICE        8
#define E_CORE_ERROR    16
#define E_CORE_WARNING  32
#define E_COMPILE_ERROR 64
#define E_COMPILE_WARNING 128
#define E_USER_ERROR    256
#define E_USER_WARNING  512
#define E_USER_NOTICE   1024
#define E_STRICT        2048
#define E_RECOVERABLE_ERROR 4096
#define E_DEPRECATED    8192
#define E_USER_DEPRECATED 16384
```

---

## Module Lifecycle Functions

```c
// Module startup (called once when module is loaded)
PHP_MINIT_FUNCTION(module_name)
{
    // Register classes, constants, INI entries
    return SUCCESS;
}

// Module shutdown (called once when module is unloaded)
PHP_MSHUTDOWN_FUNCTION(module_name)
{
    // Clean up global resources
    return SUCCESS;
}

// Request startup (called at the start of each request)
PHP_RINIT_FUNCTION(module_name)
{
    // Initialize per-request data
    return SUCCESS;
}

// Request shutdown (called at the end of each request)
PHP_RSHUTDOWN_FUNCTION(module_name)
{
    // Clean up per-request data
    return SUCCESS;
}

// Module info (called by phpinfo())
PHP_MINFO_FUNCTION(module_name)
{
    php_info_print_table_start();
    php_info_print_table_header(2, "Module", "Enabled");
    php_info_print_table_row(2, "Version", PHP_MODULE_VERSION);
    php_info_print_table_end();
}
```

---

## Common Extension Types

### 1. Function Extensions
Provide additional PHP functions (e.g., `json`, `hash`, `bcmath`)
- Implement functions in C
- Register in `zend_function_entry` array
- Simple lifecycle management

### 2. Class Extensions
Provide PHP classes with methods (e.g., `DateTime`, `DOMDocument`)
- Register class entries
- Implement methods as functions
- Handle object creation and destruction

### 3. SAPI Extensions
Server API modules (e.g., `cli`, `fpm`, `apache2handler`)
- Handle PHP execution in different environments
- Implement request handling
- Manage server-specific features

### 4. Filter Extensions
Provide stream filters (e.g., `zlib`, `mcrypt`)
- Register filter operations
- Implement filter chains
- Handle stream processing

---

## Build System

### Autoconf (Unix/Linux/macOS)
- `configure.ac` - Main configuration
- `config.m4` - Extension-specific configuration
- `acinclude.m4` - Shared macros

### CMake (Windows)
- `CMakeLists.txt` - Main configuration
- `config.w32` - Extension-specific configuration

### Config.m4 Example
```m4
PHP_ARG_ENABLE(json, whether to enable JSON support,
[  --enable-json          Enable JSON support])

if test "$PHP_JSON" != "no"; then
    PHP_REQUIRE_CC()
    PHP_ADD_LIBRARY(stdc++, 1, JSON_SHARED_LIBADD)
    PHP_NEW_EXTENSION(jsonext, json.c php_json.c, $ext_shared)
fi
```

---

## Debugging and Development Tips

### Compile PHP with Debug Symbols
```bash
cd "${SCRATCHPAD}/.php-extensions/php-src"
./configure --enable-debug
make
```

### Run Tests
```bash
cd "${SCRATCHPAD}/.php-extensions/php-src"

# Run all tests
make test

# Run specific extension tests
cd ext/json && make test

# Run single test
php -n -d extension=json.so run-tests.php tests/bug_12345.phpt
```

### PHPT Test Format
```phpt
--TEST--
Test description
--SKIPIF--
<?php if (!extension_loaded('json')) die('skip'); ?>
--FILE--
<?php
// Test code
echo json_encode(['a' => 'b']);
--EXPECT--
{"a":"b"}
```

---

## Version-Specific Information

### PHP Version Macros
```c
#define PHP_VERSION "8.5.0"
#define PHP_MAJOR_VERSION 8
#define PHP_MINOR_VERSION 5
#define PHP_RELEASE_VERSION 0
#define PHP_VERSION_ID 80500

// Check PHP version
#if PHP_VERSION_ID >= 80500
// PHP 8.5+
#elif PHP_VERSION_ID >= 80400
// PHP 8.4
#elif PHP_VERSION_ID >= 80000
// PHP 8.0+
#elif PHP_VERSION_ID >= 70400
// PHP 7.4
#endif
```

### Zend Engine API Version
```c
#define ZEND_MODULE_API_NO 20230817  // PHP 8.4 (check PHP-8.5 branch for latest)
#define ZEND_EXTENSION_API_NO 20230817
```

**List of Zend API versions by PHP version:**
- PHP 8.5: `TBD` (check `Zend/zend.h` in PHP-8.5 branch)
- PHP 8.4: `20230817`
- PHP 8.3: `20230817`
- PHP 8.2: `20220829`
- PHP 8.1: `20210902`
- PHP 8.0: `20200902`
- PHP 7.4: `20190902`

---

## Resources

### Online Resources
- [PHP Internals Book](https://www.phpinternalsbook.com/)
- [PHP Source Repository](https://github.com/php/php-src)
- [PHP Wiki](https://wiki.php.net/)
- [PHP RFCs](https://wiki.php.net/rfc)
- [PHP Manual - Internals](https://www.php.net/manual/en/internals2.php)

### Mailing Lists
- internals@lists.php.net - PHP Internals Discussion
- php-dev@lists.php.net - PHP Development

### Documentation in Repository
- `docs/` directory in php-src
- `README*` files in extension directories
- Code comments (often extensive in core files)

---

## Quick Reference Commands

All commands should be run from: `${SCRATCHPAD}/.php-extensions/php-src/`

| Question | Command |
|----------|---------|
| Initialize repository | `git clone https://github.com/php/php-src.git .php-extensions/php-src` |
| Update repository | `git pull origin master` |
| Find function implementation | `grep -r "PHP_FUNCTION(name)" ext/` |
| Find internal function | `grep -r "zif_name" ext/` |
| Find class definition | `grep -r "zend_class_entry.*Class" ext/` |
| Find INI setting | `grep -r "PHP_INI_ENTRY.*name" ext/` |
| Find macro definition | `grep -r "#define NAME" Zend/ include/` |
| Find structure definition | `grep -r "typedef struct.*_name" Zend/ include/` |
| Find where symbol is used | `grep -r "symbol_name" .` |
| List all extensions | `ls ext/` |
| List SAPI modules | `ls sapi/` |
| Switch PHP version | `git checkout PHP-8.5` |

---

## Execution Instructions

When invoked with a PHP extension-related question:

1. **Initialize the repository** (if not already present):
   - Set `PHP_SRC_DIR="${SCRATCHPAD}/.php-extensions/php-src"`
   - If directory doesn't exist, clone php-src from GitHub
   - Ensure repository is on the master branch

2. **Analyze the question** to determine which component it relates to (extension, Zend Engine, SAPI, core)

3. **Identify the relevant files** using the patterns and paths described above

4. **Navigate to repository** and use grep/bash commands to locate specific implementations

5. **Read the source code** using the read_file tool with absolute paths from `${SCRATCHPAD}/.php-extensions/php-src/`

6. **Search for specific implementations** using grep patterns from the repository root

7. **Synthesize the information** into a clear, accurate answer

8. **Cite specific files and line numbers** relative to the php-src repository

9. **Explain the implementation** in the context of PHP internals

**Remember:**
- All file paths for read_file should use: `${SCRATCHPAD}/.php-extensions/php-src/path/to/file`
- All grep/bash commands should be run from: `${SCRATCHPAD}/.php-extensions/php-src/`
- The PHP source code is the authoritative source for extension behavior
- Comments in the source often explain non-obvious implementation details
- Tests in `ext/*/tests/` demonstrate expected behavior
- The Zend Engine API changes between major PHP versions
- This skill supports Windows, Linux, and macOS through cross-platform commands

---

## Troubleshooting

### Repository Issues

**Problem: Repository not found**
```bash
# Re-clone the repository
cd "${SCRATCHPAD}"
rm -rf .php-extensions
mkdir -p .php-extensions
git clone https://github.com/php/php-src.git .php-extensions/php-src
```

**Problem: Git not available**
- Ensure Git is installed on your system
- Windows: Install Git for Windows from https://git-scm.com/
- Linux: `sudo apt install git` (Debian/Ubuntu) or `sudo dnf install git` (Fedora/RHEL)
- macOS: `brew install git` or install Xcode command line tools

**Problem: Permission denied**
```bash
# On Linux/macOS
chmod -R u+rw "${SCRATCHPAD}/.php-extensions"
```

### Cross-Platform Issues

**Path format issues:**
- Use forward slashes (`/`) in all commands - they work on all platforms
- The `${SCRATCHPAD}` environment variable will have the correct format for the current platform
- When using read_file, always use the absolute path format provided by the system

**Line ending issues:**
- Git handles line endings automatically
- If you see `^M` characters, use `dos2unix` on Linux/macOS or configure Git:
  ```bash
  git config --global core.autocrlf input
  ```

---

*Last updated: PHP 8.5 era*
