cmake_minimum_required(VERSION 2.8...3.27)

include(FetchContent)

project(ogl_imgui_sample)

set(CMAKE_CXX_STANDARD 17)

add_executable(ogl_imgui_sample main.cpp)

find_package(OpenGL REQUIRED)

FetchContent_Declare(glad
        GIT_REPOSITORY "https://github.com/Dav1dde/glad.git"
        GIT_TAG "1ecd45775d96f35170458e6b148eb0708967e402"
)
FetchContent_MakeAvailable(glad)

FetchContent_Declare(glfw
        GIT_REPOSITORY "https://github.com/glfw/glfw"
        GIT_TAG "3.3.9"
        GIT_SHALLOW ON
)
FetchContent_MakeAvailable(glfw)

FetchContent_Declare(imgui
        GIT_REPOSITORY "https://github.com/ocornut/imgui.git"
        GIT_TAG "a3eea8a75a073694df72a31530632804df0001ec"
)
FetchContent_GetProperties(imgui)
if (NOT imgui_POPULATED)
    FetchContent_Populate(imgui)
    set(IMGUI_INCLUDE_DIR ${imgui_SOURCE_DIR}/)
    file(GLOB IMGUI_SOURCES ${imgui_SOURCE_DIR}/*.cpp)
    file(GLOB IMGUI_HEADERS ${imgui_SOURCE_DIR}/*.h)
    add_library(imgui STATIC ${IMGUI_SOURCES} ${IMGUI_SOURCES} ${imgui_SOURCE_DIR}/backends/imgui_impl_glfw.cpp ${imgui_SOURCE_DIR}/backends/imgui_impl_opengl3.cpp)
    target_include_directories(imgui PUBLIC ${IMGUI_INCLUDE_DIR} ${imgui_SOURCE_DIR}/backends/ ${OPENGL_INCLUDE_DIR} ${GLFW_INCLUDE_DIR} ${GLAD_INCLUDE_DIR})
    target_link_libraries(imgui ${OPENGL_LIBRARIES} glfw glad)
endif ()

target_link_libraries(ogl_imgui_sample PRIVATE imgui)
