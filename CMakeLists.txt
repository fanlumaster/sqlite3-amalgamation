cmake_minimum_required(VERSION 3.25)
project(sqlite3 VERSION 3.49.1 LANGUAGES C CXX)

set(SQLITE3_HEADERS 
    "./src/sqlite3.h"
)

set(SQLITE3_SOURCES
    "./src/sqlite3.c"
)

add_library(sqlite3 STATIC ${SQLITE3_SOURCES})
add_library(SQLite3::sqlite3 ALIAS sqlite3)

target_include_directories(sqlite3 PUBLIC
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/src>
    $<INSTALL_INTERFACE:include>
)

set_target_properties(sqlite3 PROPERTIES
    ARCHIVE_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/lib"
    LIBRARY_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/lib"
    RUNTIME_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/bin"
)

install(TARGETS sqlite3 
    EXPORT SQLite3Targets
    ARCHIVE DESTINATION lib
)
install(FILES ${SQLITE3_HEADERS} DESTINATION include)
install(EXPORT SQLite3Targets 
    NAMESPACE SQLite3::
    DESTINATION lib/cmake/SQLite3
)

# Generate and install CMake config files
include(CMakePackageConfigHelpers)
write_basic_package_version_file(
    "${CMAKE_CURRENT_BINARY_DIR}/SQLite3ConfigVersion.cmake"
    VERSION ${PROJECT_VERSION}
    COMPATIBILITY SameMajorVersion
)

configure_package_config_file(
    "${CMAKE_CURRENT_SOURCE_DIR}/cmake/SQLite3Config.cmake.in"
    "${CMAKE_CURRENT_BINARY_DIR}/SQLite3Config.cmake"
    INSTALL_DESTINATION lib/cmake/SQLite3
)

install(FILES 
    "${CMAKE_CURRENT_BINARY_DIR}/SQLite3Config.cmake"
    "${CMAKE_CURRENT_BINARY_DIR}/SQLite3ConfigVersion.cmake"
    DESTINATION lib/cmake/SQLite3
)

# Geneate and install pkg-config file
configure_file(
    "${CMAKE_CURRENT_SOURCE_DIR}/cmake/sqlite3.pc.in"
    "${CMAKE_CURRENT_BINARY_DIR}/sqlite3.pc" @ONLY
)
install(FILES "${CMAKE_CURRENT_BINARY_DIR}/sqlite3.pc"
    DESTINATION lib/pkgconfig)