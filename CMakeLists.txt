cmake_minimum_required( VERSION 3.3 )
project( test_clion_terminal_ndk )

set( CMAKE_VERBOSE_MAKEFILE ON )

set( CMAKE_C_COMPILER_WORKS   1 )
set( CMAKE_CXX_COMPILER_WORKS 1 )

# set this to wherever android NDK lives on your computer
set( NDK_ROOT "/Users/r/code/android/android-ndk/android-ndk-r10e" )

set( NDK_PLATFORM 21    )
set( NDK_ARCH     "armeabi-v7a" )
set( NDK_TOOLCHAIN "gcc49" )
set( NDK_STL "gnustl" )

set( NDK_PLATFORM_DIR "android-${NDK_PLATFORM}" )

set( NDK_TOOLCHAIN_DIR "arm-linux-androideabi-4.9" )
set( NDK_TOOLCHAIN_PREFIX "arm-linux-androideabi" )

set( NKD_ARCH_OUT "armeabi-v7a" )

# STL
include_directories(
	${NDK_ROOT}/sources/cxx-stl/gnu-libstdc++/4.9/include
	${NDK_ROOT}/sources/cxx-stl/gnu-libstdc++/4.9/include/backward
	${NDK_ROOT}/sources/cxx-stl/gnu-libstdc++/4.9/libs/armeabi-v7a/include
)

# Toolchain host
if( APPLE )
	set( NDK_TOOLCHAIN_HOST "darwin-x86_64" )
elseif( WINDOWS )
elseif( UNIX )
	set( NDK_TOOLCHAIN_HOST "linux-x86_64" )
endif()

# Toolchain prefix
set( NKD_TOOLCHAIN_ROOT "${NDK_ROOT}/toolchains/${NDK_TOOLCHAIN_DIR}/prebuilt/${NDK_TOOLCHAIN_HOST}" )

set( NDK_STDLIB_PATH "${NDK_ROOT}/platforms/${NDK_PLATFORM_DIR}/arch-arm/usr/lib" )

# sysroot
set( NDK_SYSROOT "${NDK_ROOT}/platforms/${NDK_PLATFORM_DIR}/arch-arm" )
set( CMAKE_SYSROOT ${NDK_SYSROOT} CACHE FILEPATH "" FORCE )
if( APPLE )
	set( CMAKE_OSX_SYSROOT "" CACHE FILEPATH "" FORCE )
endif()

# Compiler, build tool settings
set( CMAKE_C_COMPILER   ${NKD_TOOLCHAIN_ROOT}/bin/${NDK_TOOLCHAIN_PREFIX}-gcc    CACHE FILEPATH "" FORCE )
set( CMAKE_CXX_COMPILER ${NKD_TOOLCHAIN_ROOT}/bin/${NDK_TOOLCHAIN_PREFIX}-g++    CACHE FILEPATH "" FORCE )
set( CMAKE_AR           ${NKD_TOOLCHAIN_ROOT}/bin/${NDK_TOOLCHAIN_PREFIX}-ar     CACHE FILEPATH "" FORCE )
set( CMAKE_LINKER       ${NKD_TOOLCHAIN_ROOT}/bin/${NDK_TOOLCHAIN_PREFIX}-ld     CACHE FILEPATH "" FORCE )
set( CMAKE_NM           ${NKD_TOOLCHAIN_ROOT}/bin/${NDK_TOOLCHAIN_PREFIX}-nm     CACHE FILEPATH "" FORCE )
set( CMAKE_RANLIB       ${NKD_TOOLCHAIN_ROOT}/bin/${NDK_TOOLCHAIN_PREFIX}-ranlib CACHE FILEPATH "" FORCE )
set( CMAKE_STRIP        ${NKD_TOOLCHAIN_ROOT}/bin/${NDK_TOOLCHAIN_PREFIX}-strip  CACHE FILEPATH "" FORCE )

# print vars
message( STATUS "NDK_STDLIB_PATH : ${NDK_STDLIB_PATH}" )
message( STATUS "LIBRARY_OUTPUT_PATH: ${LIBRARY_OUTPUT_PATH}" )
message( STATUS "" )
message( STATUS "CMAKE_C_COMPILER  : ${CMAKE_C_COMPILER}" )
message( STATUS "CMAKE_CXX_COMPILER: ${CMAKE_CXX_COMPILER}" )
message( STATUS "CMAKE_AR          : ${CMAKE_AR}" )
message( STATUS "CMAKE_LINKER      : ${CMAKE_LINKER}" )
message( STATUS "CMAKE_NM          : ${CMAKE_NM}" )
message( STATUS "CMAKE_RANLIB      : ${CMAKE_RANLIB}" )
message( STATUS "CMAKE_STRIP       : ${CMAKE_STRIP}" )
message( STATUS "" )
message( STATUS "CMAKE_SYSROOT     : ${CMAKE_SYSROOT}" )
message( STATUS "" )

set( CMAKE_C_FLAGS "-Wall -Wextra -Werror" )
set( CMAKE_CXX_FLAGS "${CXX_FLAGS} -Wall -Wextra -Werror -std=c++11" )

add_executable( test_clion_terminal_ndk main.cpp )

link_directories( NDK_STDLIB_PATH )
target_link_libraries( test_clion_terminal_ndk stdc++ )
