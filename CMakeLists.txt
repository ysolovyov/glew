cmake_minimum_required(VERSION 3.11)
project(GLEW LANGUAGES C)

find_package(OpenGL REQUIRED)

set(GLEW_LIBRARIES ${OPENGL_LIBRARIES})

option (GLEW_X11 "X11 mode" ON)

# X11 required except for Windows and Apple OSX platforms
if(GLEW_X11 AND NOT WIN32 AND NOT APPLE)
    find_package(X11)
    list(APPEND GLEW_LIBRARIES ${OPENGL_glx_LIBRARY} ${X11_LIBRARIES})
endif()

set(GLEW_PUBLIC_HEADERS
    include/GL/eglew.h include/GL/glew.h include/GL/glxew.h include/GL/wglew.h)
 
add_library(glew STATIC ${GLEW_PUBLIC_HEADERS} src/glew.c)
target_include_directories(glew PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}/include)
target_link_libraries(glew ${OPENGL_LIBRARIES})
target_compile_definitions(glew PUBLIC GLEW_STATIC)

add_library(glew::glew ALIAS glew)