cmake_minimum_required(VERSION 3.18)
project(Skyrim_iOS)

set(CMAKE_SYSTEM_NAME iOS)
set(CMAKE_OSX_SYSROOT iphoneos)
set(CMAKE_OSX_ARCHITECTURES arm64)
set(CMAKE_C_STANDARD 11)
set(CMAKE_CXX_STANDARD 17)

set(SDL_SHARED OFF CACHE BOOL "" FORCE)
set(SDL_STATIC ON CACHE BOOL "" FORCE)
set(SDL_TEST OFF CACHE BOOL "" FORCE)
set(SDL_VIDEO OFF CACHE BOOL "" FORCE)
set(SDL_AUDIO ON CACHE BOOL "" FORCE)
set(SDL_JOYSTICK ON CACHE BOOL "" FORCE)
set(SDL_HAPTIC OFF CACHE BOOL "" FORCE)
set(SDL_POWER ON CACHE BOOL "" FORCE)
set(SDL_THREADS ON CACHE BOOL "" FORCE)
set(SDL_FILESYSTEM ON CACHE BOOL "" FORCE)

add_subdirectory(SDL2 EXCLUDE_FROM_ALL)

set(GEN_DIR ../gen)
set(RUNTIME_DIR ../runtime)

file(GLOB GEN_SOURCES ${GEN_DIR}/*.c)
file(GLOB RUNTIME_SOURCES ${RUNTIME_DIR}/*.c ${RUNTIME_DIR}/*.cpp)

add_executable(Skyrim ${GEN_SOURCES} ${RUNTIME_SOURCES})

target_include_directories(Skyrim PRIVATE
        ${GEN_DIR}
        ${RUNTIME_DIR}
        dxvk/include/native/directx
        dxvk/include/native/windows
        SDL2/include
)

target_compile_definitions(Skyrim PRIVATE GAME_SKYRIM DXVK_NATIVE)
target_compile_options(Skyrim PRIVATE -O2 -g -fno-strict-aliasing -Wno-everything)

target_link_libraries(Skyrim PRIVATE
        SDL2-static
        ${CMAKE_CURRENT_SOURCE_DIR}/MoltenVK.xcframework/ios-arm64/libMoltenVK.a
)

target_link_libraries(Skyrim PRIVATE
        "-framework Foundation"
        "-framework UIKit"
        "-framework Metal"
        "-framework QuartzCore"
        "-framework CoreGraphics"
        "-framework GameController"
        "-framework AudioToolbox"
        "-framework AVFoundation"
        "-framework CoreMotion"
)