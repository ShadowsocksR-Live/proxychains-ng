cmake_minimum_required(VERSION 3.0)
project(proxychains4 C)

set(CMAKE_C_STANDARD 11)

if(ANDROID)
    include_directories(${LIB_ANCILLARY_DIR})

    add_definitions(-DDLL_NAME="libproxychains4.so")
    add_definitions(-DLIB_DIR="/data/user/0/com.ssrlive.ssrdroid/lib")
    add_definitions(-DINSTALL_PREFIX="/data/user/0/com.ssrlive.ssrdroid/")
    add_definitions(-DSYSCONFDIR="/data/user/0/com.ssrlive.ssrdroid/")
endif()

if(APPLE)
    add_definitions(-DIS_MAC=1)
    add_definitions(-Ds6_addr16=__u6_addr.__u6_addr16 -Ds6_addr32=__u6_addr.__u6_addr32)
endif()

include_directories(.)
include_directories(src)

set(CMAKE_C_FLAGS  "${CMAKE_C_FLAGS} -std=c99 -Wall -Wextra -O0 -g")
add_definitions(-D_GNU_SOURCE -DSUPER_SECURE)

set(PROXY_CHAINS_4_SOURCES
        src/allocator_thread.c
        src/allocator_thread.h
        src/common.c
        src/common.h
        src/core.c
        src/core.h
        src/debug.c
        src/debug.h
        src/hash.c
        src/hash.h
        src/hostsreader.c
        src/ip_type.c
        src/ip_type.h
        src/libproxychains.c
        src/version.c
        src/version.h)

add_library(proxychains4 SHARED ${PROXY_CHAINS_4_SOURCES})

if(ANDROID)
target_link_libraries(proxychains4 ancillary log dl)
endif()
