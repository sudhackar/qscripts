cmake_minimum_required(VERSION 3.27)
project(qscripts)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Include IDA SDK bootstrap
include($ENV{IDASDK}/ida-cmake/bootstrap.cmake)
find_package(idasdk REQUIRED)

# Add idacpp library
add_subdirectory($ENV{IDASDK}/include/idacpp ${CMAKE_CURRENT_BINARY_DIR}/idacpp)

# Convert paths to native format
set(SAMPLE_IDB "${IDA_CMAKE_DIR}/samples/wizmo32.exe.i64")
file(TO_NATIVE_PATH "${SAMPLE_IDB}" SAMPLE_IDB)

# Disabled sources
list(APPEND DISABLED_SOURCES utils_impl.cpp)
set_source_files_properties(${DISABLED_SOURCES} PROPERTIES LANGUAGE "")

# Add plugin
ida_add_plugin(qscripts
    SOURCES
        qscripts.cpp
        ida.h
        script.hpp
        ${DISABLED_SOURCES}
    DEBUG_ARGS
        "${SAMPLE_IDB}"
)

target_link_libraries(qscripts PRIVATE idacpp::idacpp)
