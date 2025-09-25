cmake_minimum_required(VERSION 3.16)

project(timemachine VERSION 1.0 LANGUAGES C ASM)
set(ARKSDK "${CMAKE_CURRENT_SOURCE_DIR}/../ark-dev-sdk" CACHE PATH "Path to Ark5 dev sdk")
set(BOOTLOADEX "${CMAKE_CURRENT_SOURCE_DIR}/../BootLoadEx" CACHE PATH "Path to Ark5 BootLoadEx")
set (TMCTRL_MOD_LIBS -nostartfiles -nostdlib pspsystemctrl_kernel pspmodinfo pspsdk pspkernel)

message(STATUS "ARKSDK = ${ARKSDK}")
message(STATUS "BOOTLOADEX = ${BOOTLOADEX}")

add_compile_options(-std=c99 -Os -G0 -Wall -fno-pic -fshort-wchar)
include_directories(${ARKSDK}/include)
link_directories(${ARKSDK}/libs)

add_subdirectory(TMCtrl150)
add_subdirectory(TMCtrl660)