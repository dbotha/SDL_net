cmake_minimum_required(VERSION 3.17)
project(SDL_net C)

if (NOT ANDROID AND NOT (TARGET SDL2 OR TARGET SDL2-static))
    find_package(SDL2 REQUIRED)
endif()

option(BUILD_SHARED_LIBS "Build the library as a shared library" ON)

if (NOT BUILD_SHARED_LIBS)
    set(CMAKE_POSITION_INDEPENDENT_CODE ON)
endif()

add_library(SDL2_net)

target_sources(SDL2_net PRIVATE
        SDLnet.c
        SDLnetTCP.c
        SDLnetUDP.c
        SDLnetselect.c
        SDLnetsys.h)

add_library(SDL2::net ALIAS SDL2_net)

target_include_directories(SDL2_net PUBLIC ${CMAKE_CURRENT_SOURCE_DIR})

if (BUILD_SHARED_LIBS)
    if(WIN32)
        target_compile_definitions(SDL2_net PRIVATE -DDLL_EXPORT)
    endif()
    target_link_libraries(SDL2_net PUBLIC SDL2::SDL2)
else()
    target_link_libraries(SDL2_net PUBLIC SDL2::SDL2-static)
endif()
