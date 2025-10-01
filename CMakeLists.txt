cmake_minimum_required(VERSION 3.28)

project(timemachine VERSION 1.0 LANGUAGES C ASM)
set(DEBUG "" CACHE STRING "Debug level (1,2,3)")
message(STATUS "DEBUG LEVEL = ${DEBUG}")

include(FetchContent)

FetchContent_Declare(
    ark-dev-sdk
    GIT_REPOSITORY https://github.com/PSP-Arkfive/ark-dev-sdk.git
    GIT_TAG main
    EXCLUDE_FROM_ALL
)

FetchContent_Declare(
    bootloadex
    GIT_REPOSITORY https://github.com/PSP-Arkfive/BootLoadEx.git
    GIT_TAG main
    EXCLUDE_FROM_ALL
)

FetchContent_MakeAvailable(ark-dev-sdk bootloadex)

set(ARKSDK ${ark-dev-sdk_SOURCE_DIR})
set(BOOTLOADEX ${bootloadex_SOURCE_DIR})

set(TMCTRL_MOD_LIBS -nostartfiles -nostdlib pspsystemctrl_kernel pspmodinfo pspsdk pspkernel)
add_compile_options(-std=c99 -Os -G0 -Wall -fno-pic -fshort-wchar)
include_directories(${ARKSDK}/include)
link_directories(${ARKSDK}/libs)

add_subdirectory(TMCtrl150)
add_subdirectory(TMCtrl660)