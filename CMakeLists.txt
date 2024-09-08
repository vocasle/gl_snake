cmake_minimum_required(VERSION 3.20)
project(snake_gl)

set(CMAKE_CXX_STANDARD 20)

set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${CMAKE_SOURCE_DIR}/cmake/modules/")

include(FetchContent)
FetchContent_Declare(
    glfw
    GIT_REPOSITORY https://github.com/glfw/glfw.git
    GIT_TAG 7b6aead9fb88b3623e3b3725ebb42670cbe4c579
    OVERRIDE_FIND_PACKAGE)
FetchContent_MakeAvailable(glfw)


FetchContent_Declare(
    glm
    GIT_REPOSITORY https://github.com/g-truc/glm.git
    GIT_TAG 0af55ccecd98d4e5a8d1fad7de25ba429d60e863
    OVERRIDE_FIND_PACKAGE)

FetchContent_MakeAvailable(glm)

find_package(glfw REQUIRED)
find_package(glm REQUIRED)
message(STATUS "GLM included at ${GLM_INCLUDE_DIR}")

set(LIBS OpenGL::GL glad dl stb_image minaudio glfw glm)

find_package(OpenGL REQUIRED)
add_definitions(${OPENGL_DEFINITIONS})

add_subdirectory(thirdparties)

add_executable(snake_gl src/main.cpp)

target_link_libraries(snake_gl PRIVATE ${LIBS})
target_include_directories(snake_gl PRIVATE ${CMAKE_SOURCE_DIR}/include)
set_target_properties(snake_gl PROPERTIES RUNTIME_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/${CHAPTER}")

file(GLOB SHADERS
        "src/*.vs"
        "src/*.fs")

file(GLOB ASSETS
        "assets/*.jpg"
        "assets/*.png"
        "assets/*.mp3")

foreach(ASSET ${ASSETS})
    file(COPY ${ASSET} DESTINATION ${CMAKE_BINARY_DIR}/assets)
endforeach()

foreach(SHADER ${SHADERS})
    file(COPY ${SHADER} DESTINATION ${CMAKE_BINARY_DIR}/shaders)
endforeach()


install(TARGETS snake_gl DESTINATION snake)
install(FILES include/shader.h DESTINATION snake/include)
install(DIRECTORY thirdparties DESTINATION snake/thirdparties)
install(FILES ${ASSETS} DESTINATION snake/assets)
install(FILES ${SHADERS} DESTINATION snake/shaders)
