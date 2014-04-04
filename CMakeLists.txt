#
# CMake listfile to specify the build process, see:
# http://www.cmake.org/cmake/help/documentation.html
#

project(zxing)
cmake_minimum_required(VERSION 2.8.0)

set(CMAKE_LIBRARY_PATH /opt/local/lib ${CMAKE_LIBRARY_PATH})


# Suppress in-source builds.
if(${CMAKE_SOURCE_DIR} STREQUAL ${CMAKE_BINARY_DIR})
    message(FATAL_ERROR
        "CMake generation is not allowed within the source directory:"
        "\n  * mkdir build"
        "\n  * cd build"
        "\n  * Unix-like: cmake -G \"Unix Makefiles\" .."
        "\n  * Windows: cmake -G \"Visual Studio 10\" ..")
endif()

# Adjust CMake's module path.

set(CMAKE_MODULE_PATH ${CMAKE_SOURCE_DIR}/cmake/)

# Suppress MSVC CRT warnings.
if(MSVC)
    foreach(flag_var CMAKE_CXX_FLAGS CMAKE_CXX_FLAGS_DEBUG CMAKE_CXX_FLAGS_RELEASE
        CMAKE_CXX_FLAGS_MINSIZEREL CMAKE_CXX_FLAGS_RELWITHDEBINFO)
       if(${flag_var} MATCHES "/MD")
         string(REGEX REPLACE "/MD" "/MT" ${flag_var} "${${flag_var}}")
       endif(${flag_var} MATCHES "/MD")
    endforeach(flag_var)
    add_definitions(-D_CRT_SECURE_NO_WARNINGS)
    add_definitions(-Zc:wchar_t-)
	add_definitions(/W1)
	add_definitions(/MP)
endif()

# Add libzxing library.
file(GLOB_RECURSE LIBZXING_FILES
    "./src/libsrc/*.cpp"
    "./src/libsrc/*.h"
    "./src/libsrc/*.cc"
    "./src/libsrc/*.hh"
)
if(WIN32)
    file(GLOB LIBZXING_WIN32_FILES
        "./src/libsrc/win32/zxing/*.c"
        "./src/libsrc/win32/zxing/*.h"
    )
    set(LIBZXING_FILES ${LIBZXING_FILES} ${LIBZXING_WIN32_FILES})
    include_directories(SYSTEM "./src/libsrc/win32/zxing/")
endif()
include_directories("./src/libsrc/")
add_library(libzxing STATIC ${LIBZXING_FILES})
set_target_properties(libzxing PROPERTIES PREFIX "")
if(!WIN32)
  find_package(Iconv)
  if(ICONV_FOUND)
    include_directories(${ICONV_INCLUDE_DIR})
    target_link_libraries(libzxing ${ICONV_LIBRARIES})
  else()
    add_definitions(-DNO_ICONV=1)
  endif()
endif()

# Add cli executable.
file(GLOB_RECURSE ZXING_FILES
    "./src/cli/src/*.cpp"
    "./src/cli/src/*.h"
)
add_executable(zxing ${ZXING_FILES})
target_link_libraries(zxing libzxing)

