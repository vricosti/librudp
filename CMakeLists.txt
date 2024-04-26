cmake_minimum_required(VERSION 3.10)
project(librudp VERSION 0.3 DESCRIPTION "Reliable UDP library" LANGUAGES C)

# Compiler settings
add_compile_options(-Wstrict-prototypes -Wmissing-prototypes -Wno-unused-parameter)

# Attempt to find libela in a sibling directory
list(APPEND CMAKE_PREFIX_PATH "${CMAKE_SOURCE_DIR}/../libela")
find_package(ela QUIET)

if(NOT ela_FOUND)
    find_package(ela REQUIRED)
endif()

# Include directories
include_directories(include ${ela_INCLUDE_DIRS})

# Source files
set(SOURCE_FILES
    src/address.c
    src/client.c
    src/endpoint.c
    src/packet.c
    src/peer.c
    src/rudp.c
    src/server.c)

# Define the library target
add_library(rudp STATIC ${SOURCE_FILES})
target_link_libraries(rudp PUBLIC ela)
set_target_properties(rudp PROPERTIES PUBLIC_HEADER include/rudp.h)

# Installation rules
install(TARGETS rudp
    ARCHIVE DESTINATION lib
    PUBLIC_HEADER DESTINATION include)

# Optionally create a pkg-config file
include(FindPkgConfig)
if(PKG_CONFIG_FOUND)
    configure_file(${CMAKE_SOURCE_DIR}/rudp.pc.in ${CMAKE_BINARY_DIR}/rudp.pc @ONLY)
    install(FILES ${CMAKE_BINARY_DIR}/rudp.pc DESTINATION lib/pkgconfig)
endif()
