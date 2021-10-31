cmake_minimum_required(VERSION 3.0 FATAL_ERROR)

project(LUA VERSION 5.4.3 LANGUAGES C)

option(BUILD_SHARED_LIBS "Build shared libraries" OFF)
option(LUA_BUILD_COMPILER "Build the Lua bytecode compiler" ON)
option(LUA_BUILD_EXECUTABLE "Build the Lua executable" ON)
option(LUA_INSTALL "Generate installation targets" ON)
option(LUA_USE_C89 "Forces Lua to use C89 standard" OFF)
option(LUA_USE_POSIX "Forces Lua to use POSIX standard" OFF)
option(LUA_USE_DL "Use dynamic loading mechanism" OFF)
option(LUA_32BITS "Use 32 bit number in Lua" OFF)
option(LUA_COMPAT_5_3 "Use Lua 5.3 compatability" ON)

if(BUILD_SHARED_LIBS AND WIN32)
    set(LUA_BUILD_AS_DLL 1)
endif()

# Add subdirectories
add_subdirectory(src)
