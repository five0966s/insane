cmake_minimum_required(VERSION 3.20) [cite: 1]
project(Insane_tf2 LANGUAGES CXX) [cite: 1]
set(CMAKE_EXPORT_COMPILE_COMMANDS ON) [cite: 1]

set(CMAKE_CXX_STANDARD 20) [cite: 1]
set(CMAKE_CXX_STANDARD_REQUIRED ON) [cite: 1]

file(GLOB_RECURSE SOURCES "src/*.cpp" "src/*.cxx") [cite: 1]
source_group(
    TREE "${CMAKE_CURRENT_SOURCE_DIR}/src"
    FILES ${SOURCES}
) [cite: 1]

add_library(Insane_tf2 SHARED ${SOURCES}) [cite: 1]

# Project must be built with MSVC (cl.exe). 
if(NOT MSVC) [cite: 1]
  message(FATAL_ERROR "This project must be built with MSVC (cl.exe).") [cite: 1]
endif() [cite: 1]

#//////////////////////////////////////////////////
# Linking DirectX9 library (Flexible Path for GitHub)
#//////////////////////////////////////////////////
target_link_libraries(Insane_tf2 PRIVATE d3d9 d3dx9) [cite: 1]

# This check allows the build to work on both GitHub (C:/dxsdk) and your local PC 
if(EXISTS "C:/dxsdk")
    set(DX_PATH "C:/dxsdk")
else()
    set(DX_PATH "C:/Program Files (x86)/Microsoft DirectX SDK (June 2010)")
endif()

target_include_directories(Insane_tf2 PRIVATE "${DX_PATH}/Include") [cite: 1]
target_link_directories(Insane_tf2 PRIVATE "${DX_PATH}/Lib/x64") [cite: 1]

#//////////////////////////////////////////////////
# Linking MinHook.lib x64 
#//////////////////////////////////////////////////
target_link_libraries(Insane_tf2 PRIVATE "${CMAKE_SOURCE_DIR}/includes/MinHook.lib") [cite: 1]

file(MAKE_DIRECTORY "${CMAKE_SOURCE_DIR}/Build") [cite: 1]
file(MAKE_DIRECTORY "${CMAKE_SOURCE_DIR}/Output") [cite: 1]
file(MAKE_DIRECTORY "${CMAKE_SOURCE_DIR}/Output/Debug") [cite: 1]
file(MAKE_DIRECTORY "${CMAKE_SOURCE_DIR}/Output/Release") [cite: 1]

set_target_properties(Insane_tf2 PROPERTIES
    RUNTIME_OUTPUT_DIRECTORY_DEBUG   "${CMAKE_SOURCE_DIR}/Output/Debug"
    RUNTIME_OUTPUT_DIRECTORY_RELEASE "${CMAKE_SOURCE_DIR}/Output/Release"
) [cite: 1]

target_compile_options(Insane_tf2 PRIVATE /SUBSYSTEM:WINDOWS ) [cite: 1]
