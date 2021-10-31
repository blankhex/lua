# Lua
This is Lua 5.4.3, released on 15 Mar 2021.

For installation instructions, license details, and
further information about Lua, see doc/readme.html.

Modified by blankhex to work with CMake build system.
Beware! There may be bugs!

# Building
## Default
```
mkdir build
cmake ..
cmake --build
```

## Library only
```
mkdir build
cmake -DLUA_BUILD_COMPILER=0 -DLUA_BUILD_EXECUTABLE=0 ..
cmake --build
```

# Configuration options
- `BUILD_SHARED_LIBS`: Build shared libraries, `OFF` by default
- `LUA_BUILD_COMPILER`: Build the Lua bytecode compiler, `ON` by default
- `LUA_BUILD_EXECUTABLE`: Build the Lua executable, `ON` by default
- `LUA_INSTALL`: Generate installation targets, `ON` by default
- `LUA_USE_C89`: Forces Lua to use C89 standard, `OFF` by default
- `LUA_USE_POSIX`: Forces Lua to use POSIX standard, `OFF` by default
- `LUA_USE_DL`: Use dynamic loading mechanism, `OFF` by default
- `LUA_32BITS`: Use 32 bit numbers in Lua, `OFF` by default
- `LUA_COMPAT_5_3`: Use Lua 5.3 compatability, `ON` by default
