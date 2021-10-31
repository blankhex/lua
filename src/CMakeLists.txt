include(GNUInstallDirs)

# Create generated files
include(CMakePackageConfigHelpers)

set(LUA_CONFIG_PATH "${CMAKE_INSTALL_LIBDIR}/cmake/lua5.4")

configure_package_config_file(luaConfig.cmake.in
                              luaConfig.cmake
                              INSTALL_DESTINATION "${LUA_CONFIG_PATH}"
                              NO_CHECK_REQUIRED_COMPONENTS_MACRO
)

write_basic_package_version_file(luaConfigVersion.cmake
                                 VERSION ${LUA_VERSION}
                                 COMPATIBILITY SameMinorVersion
)


configure_file(${CMAKE_CURRENT_SOURCE_DIR}/luaconf.h.in ${CMAKE_CURRENT_BINARY_DIR}/luaconf.h)
include_directories(${CMAKE_CURRENT_SOURCE_DIR} ${CMAKE_CURRENT_BINARY_DIR})

set(LUA_SOURCES
    lapi.c
    lauxlib.c
    lbaselib.c
    lcode.c
    lcorolib.c
    lctype.c
    ldblib.c
    ldebug.c
    ldo.c
    ldump.c
    lfunc.c
    lgc.c
    linit.c
    liolib.c
    llex.c
    lmathlib.c
    lmem.c
    loadlib.c
    lobject.c
    lopcodes.c
    loslib.c
    lparser.c
    lstate.c
    lstring.c
    lstrlib.c
    ltable.c
    ltablib.c
    ltm.c
    lundump.c
    lutf8lib.c
    lvm.c
    lzio.c
)

set(LUA_PUBLIC_HEADERS
    ${CMAKE_CURRENT_BINARY_DIR}/luaconf.h
    lua.h
    lua.hpp
    lualib.h
    lauxlib.h
)

set(LUA_PRIVATE_HEADERS
    lapi.h
    lcode.h
    lctype.h
    ldebug.h
    ldo.h
    lfunc.h
    lgc.h
    ljumptab.h
    llex.h
    llimits.h
    lmem.h
    lobject.h
    lopcodes.h
    lopnames.h
    lparser.h
    lprefix.h
    lstate.h
    lstring.h
    ltable.h
    ltm.h
    lundump.h
    lvm.h
    lzio.h
)

add_library(lua5.4 ${LUA_SOURCES} ${LUA_PUBLIC_HEADERS} ${LUA_PRIVATE_HEADERS})
target_link_libraries(lua5.4 PRIVATE m)

if(LUA_USE_DL)
    if(UNIX)
        target_link_libraries(lua5.4 PRIVATE dl)
    endif()
endif()


if(LUA_INSTALL)
    install(TARGETS lua5.4
            EXPORT luaTargets
            RUNTIME DESTINATION "${CMAKE_INSTALL_BINDIR}"
            ARCHIVE DESTINATION "${CMAKE_INSTALL_LIBDIR}"
            LIBRARY DESTINATION "${CMAKE_INSTALL_LIBDIR}"
    )

    install(FILES ${LUA_PUBLIC_HEADERS}
            DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/lua5.4
    )

    install(FILES "${LUA_BINARY_DIR}/src/luaConfig.cmake"
                  "${LUA_BINARY_DIR}/src/luaConfigVersion.cmake"
            DESTINATION "${LUA_CONFIG_PATH}"    
    )

    install(EXPORT luaTargets FILE luaTargets.cmake
            EXPORT_LINK_INTERFACE_LIBRARIES
            DESTINATION "${LUA_CONFIG_PATH}"
    )
endif()

if(LUA_BUILD_EXECUTABLE)
    add_executable(lua5.4-bin lua.c)
    set_target_properties(lua5.4-bin PROPERTIES OUTPUT_NAME lua5.4)
    target_link_libraries(lua5.4-bin PUBLIC lua5.4)
    if(LUA_INSTALL)
        install(TARGETS lua5.4-bin
                RUNTIME DESTINATION "${CMAKE_INSTALL_BINDIR}"
                ARCHIVE DESTINATION "${CMAKE_INSTALL_LIBDIR}"
                LIBRARY DESTINATION "${CMAKE_INSTALL_LIBDIR}"
        )
    endif()
endif()

if(LUA_BUILD_COMPILER)
    add_executable(luac5.4 luac.c ${LUA_SOURCES})
    target_link_libraries(luac5.4 PUBLIC lua5.4)
    target_link_libraries(luac5.4 PRIVATE m)
    if(LUA_INSTALL)
        install(TARGETS luac5.4 
                RUNTIME DESTINATION "${CMAKE_INSTALL_BINDIR}"
                ARCHIVE DESTINATION "${CMAKE_INSTALL_LIBDIR}"
                LIBRARY DESTINATION "${CMAKE_INSTALL_LIBDIR}"
        )
    endif()
endif()
