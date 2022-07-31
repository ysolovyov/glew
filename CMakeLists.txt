
cmake_minimum_required(VERSION 3.11)
project(GLEW LANGUAGES C)
set_property(GLOBAL PROPERTY USE_FOLDERS ON)

set(GLEW_PUBLIC_HEADERS
    include/GL/eglew.h include/GL/glew.h include/GL/glxew.h include/GL/wglew.h)
 
add_library(GLEW STATIC ${GLEW_PUBLIC_HEADERS}
    src/glew.c)
target_include_directories(GLEW PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}/include)

target_compile_definitions(GLEW PUBLIC GLEW_STATIC)

set_target_properties(
    GLEW
    PROPERTIES
        FOLDER "GLEW"
        POSITION_INDEPENDENT_CODE ON
        PUBLIC_HEADER "${GLEW_PUBLIC_HEADERS}"
        OUTPUT_NAME "glew"
        OUTPUT_NAME_DEBUG "glew_d"
        INTERFACE_INCLUDE_DIRECTORIES ${CMAKE_CURRENT_SOURCE_DIR}/include)

add_library(GLEW::glew ALIAS GLEW)

configure_file(GLEWConfig.cmake.in "${PROJECT_BINARY_DIR}/GLEWConfig.cmake" @ONLY)

install(
    FILES "${PROJECT_BINARY_DIR}/GlEWConfig.cmake"
    DESTINATION "${CMAKE_INSTALL_PREFIX}/lib/cmake/GLEW")

install(
    TARGETS GLEW
    EXPORT GLEW
    LIBRARY DESTINATION lib
    ARCHIVE DESTINATION lib
    RUNTIME DESTINATION bin
    PUBLIC_HEADER DESTINATION include/GL
)

