cmake_minimum_required(VERSION 3.10)
project(GLFW-Window-Test VERSION 1.0)

# --- GLFW: build via its own CMakeLists.txt ---
set(GLFW_BUILD_DOCS OFF CACHE BOOL "" FORCE)
set(GLFW_BUILD_TESTS OFF CACHE BOOL "" FORCE)
set(GLFW_BUILD_EXAMPLES OFF CACHE BOOL "" FORCE)
set(GLFW_INSTALL OFF CACHE BOOL "" FORCE)
add_subdirectory(glfw-3.3.2)

# --- GLEW: build via its Makefile ---
add_custom_target(glew_build
    COMMAND make
    WORKING_DIRECTORY ${CMAKE_SOURCE_DIR}/glew-2.1.0
    COMMENT "Building GLEW..."
    BYPRODUCTS ${CMAKE_SOURCE_DIR}/glew-2.1.0/lib/libGLEW.a
)

# --- Platform setup ---
set(CMAKE_C_STANDARD 11)

if(UNIX AND NOT APPLE)
    find_package(OpenGL REQUIRED)
    set(CMAKE_C_FLAGS "-lm -pthread -lX11")
endif()

if(APPLE)
    set(CMAKE_C_FLAGS "-framework Cocoa -framework OpenGL -framework IOKit -framework CoreVideo")
endif()

# --- Main executable ---
set(EXECUTABLE_OUTPUT_PATH ${CMAKE_SOURCE_DIR}/build)

file(GLOB SOURCES "main.c")

include_directories(
    ${CMAKE_SOURCE_DIR}/glfw-3.3.2/include
    ${CMAKE_SOURCE_DIR}/glew-2.1.0/include
)

add_executable(window ${SOURCES})

# Ensure GLEW is built before linking
add_dependencies(window glew_build)

if(UNIX AND NOT APPLE)
    target_link_libraries(window PRIVATE ${CMAKE_DL_LIBS})
    target_link_libraries(window PRIVATE OpenGL::GL)
endif()

target_link_libraries(window PRIVATE glfw)
target_link_libraries(window PRIVATE ${CMAKE_SOURCE_DIR}/glew-2.1.0/lib/libGLEW.a)
