cmake_minimum_required(VERSION 3.14)
project(SelauraLauncher)

set(CMAKE_CXX_STANDARD 20)

# Allow Linux Compiling (Github Actions) :P
if(UNIX AND NOT WIN32)
    set(CMAKE_CXX_COMPILER x86_64-w64-mingw32-g++)
    set(CMAKE_C_COMPILER x86_64-w64-mingw32-gcc)
    set(CMAKE_LINKER x86_64-w64-mingw32-gcc)
    
    set(CMAKE_SYSTEM_NAME Windows)
    set(CMAKE_SYSTEM_VERSION 10)
endif()

set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)

set(CMAKE_RUNTIME_OUTPUT_DIRECTORY_DEBUG ${CMAKE_BINARY_DIR}/bin/debug)
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY_RELEASE ${CMAKE_BINARY_DIR}/bin/release)
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY_DEBUG ${CMAKE_BINARY_DIR}/bin/debug)
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY_RELEASE ${CMAKE_BINARY_DIR}/bin/release)
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY_DEBUG ${CMAKE_BINARY_DIR}/bin/debug)
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY_RELEASE ${CMAKE_BINARY_DIR}/bin/release)

file (GLOB_RECURSE SOURCE_FILES
    "src/*.c"
    "src/*.cpp"
    "src/*.h"
    "src/*.hpp"
)

set (RESOURCE_FILE rc/app.rc)

# Compiler Flags
if(UNIX AND NOT WIN32)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -DWIN32 -D_WINDOWS -DWINDOWS -fno-rtti -fno-exceptions")
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -DWIN32 -D_WINDOWS -DWINDOWS")

    link_libraries(
        -luser32 -lgdi32 -lcomdlg32 -lws2_32
    )
endif()

# WinRT /await Fix for MSVC
if (MSVC)
    add_compile_options(/await)
endif()

# Disable RTTI
set(CMAKE_CXX_FLAGS_RELEASE "${CMAKE_CXX_FLAGS_RELEASE} /GR- /EHsc")

add_executable(
    SelauraLauncher
    ${SOURCE_FILES}
    ${RESOURCE_FILE}
)