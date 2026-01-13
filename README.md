# Lua for C3

A **Lua runtime/binding for the C3 programming language**.
This project is an early-stage port of Lua 5.4 to C3 and is still under active development.

**Status: Experimental / Work in Progress**

* Not all Lua features are tested
* Best practices may not always be followed
* Contributions, suggestions, and improvements are welcome

---

## Requirements

* A project initialized with:

```sh
c3c init
```

---

## Installation

1. **Download or clone this repository**

```sh
git clone https://github.com/MiguelCock/lua54.c3l
```

2. **Place it inside your project’s `lib/` directory**

Your project structure should look similar to:

```
my_project/
├── lib/
│   └── lua54.c3l/
├── src/
├── project.json
└── ...
```

3. **Add the dependency to `project.json`**

```json
{
  "dependencies": [ "lua54" ]
}
```

---

## Basic Usage Example

Below is a minimal example showing how to:

* create a Lua state
* open the standard libraries
* run Lua code from C3
* handle Lua errors

```c
module lua_test;

import std::io;
import lua54;

// Custom memory allocator for Lua
fn void* lua_mem(void* ud, void* ptr, usz osize, usz nsize)
{
    if (nsize == 0) {
        free(ptr);
        return null;
    }
    if (ptr == null) {
        return malloc(nsize);
    }
    return realloc(ptr, nsize);
}

fn int main(String[] args)
{
    io::printn("Hello from C3!");

    LuaState* l = lua::newstate(&lua_mem, null);
    defer lua::close(l);

    lua::openlibs(l);

    ZString code = "print('Hello from Lua!')\n";

    if (lua::l_dostring(l, code) != lua::OK) {
        io::printfn("Lua error: %s\n", lua::tostring(l, -1));
        lua::pop(l, 1);
    }

    return 0;
}
```

---

## Notes

* A **custom allocator** is required when creating a Lua state.
* Error handling follows the Lua C API model.
* API naming and structure may change as the project matures.

---

## Contributing

Contributions are very welcome:

* Code improvements
* API design feedback
* Bug reports
* Documentation fixes

Since this is an early project, **breaking changes may occur**.

---

## License

This project is licensed under the **MIT License**, the same license used by Lua.
