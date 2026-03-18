cmake_minimum_required(VERSION 3.20)
project(Insane_tf2 LANGUAGES CXX)
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

file(GLOB_RECURSE SOURCES "src/*.cpp" "src/*.cxx")
source_group(TREE "${CMAKE_CURRENT_SOURCE_DIR}/src" FILES ${SOURCES})

add_library(Insane_tf2 SHARED ${SOURCES})

if(NOT MSVC)
  message(FATAL_ERROR "This project must be built with MSVC (cl.exe).")
endif()

#//////////////////////////////////////////////////
# Linking DirectX9 library (Corrected for GitHub)
#//////////////////////////////////////////////////
target_link_libraries(Insane_tf2 PRIVATE d3d9 d3dx9)

# If we didn't pass a path from the command line, use the default
if(NOT DXSDK_DIR)
    set(DXSDK_DIR "C:/Program Files (x86)/Microsoft DirectX SDK (June 2010)")
endif()

target_include_directories(Insane_tf2 PRIVATE "${DXSDK_DIR}/Include")
target_link_directories(Insane_tf2 PRIVATE "${DXSDK_DIR}/Lib/x64")

#//////////////////////////////////////////////////
# Linking MinHook
#//////////////////////////////////////////////////
target_link_libraries(Insane_tf2 PRIVATE "${CMAKE_SOURCE_DIR}/includes/MinHook.lib")

# Create output folders
file(MAKE_DIRECTORY "${CMAKE_SOURCE_DIR}/Build")
file(MAKE_DIRECTORY "${CMAKE_SOURCE_DIR}/Output/Release")

set_target_properties(Insane_tf2 PROPERTIES
    RUNTIME_OUTPUT_DIRECTORY_RELEASE "${CMAKE_SOURCE_DIR}/Output/Release"
)

target_compile_options(Insane_tf2 PRIVATE /SUBSYSTEM:WINDOWS)
