cmake_minimum_required(VERSION 3.28)

project(timemachine VERSION 1.0 LANGUAGES C ASM)
set(DEBUG "" CACHE STRING "Debug level (1,2,3)")
message(STATUS "DEBUG LEVEL = ${DEBUG}")


set(ARK_REPOS ark-dev-sdk BootLoadEx)

include(FetchContent)
foreach(ARK_REPO ${ARK_REPOS})
   FetchContent_Declare(
      ${ARK_REPO}
      GIT_REPOSITORY https://github.com/PSP-Arkfive/${ARK_REPO}.git
      GIT_TAG main
      EXCLUDE_FROM_ALL
   )
endforeach()

FetchContent_MakeAvailable(${ARK_REPOS})

set(ARKSDK ${ark-dev-sdk_SOURCE_DIR})
set(BOOTLOADEX ${BootLoadEx_SOURCE_DIR})

set(TMCTRL_MOD_LIBS -nostartfiles -nostdlib pspsystemctrl_kernel pspmodinfo pspsdk pspkernel)
add_compile_options(-std=c99 -Os -G0 -Wall -fno-pic -fshort-wchar)
include_directories(${ARKSDK}/include)
link_directories(${ARKSDK}/libs)

add_subdirectory(TMCtrl150)
add_subdirectory(TMCtrl660)