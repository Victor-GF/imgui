cmake_minimum_required(VERSION 3.16)
project(ImGui)

set(IMGUI_SOURCES
    imgui.cpp imgui.h
    imconfig.h 
    imgui_internal.h
    imstb_rectpack.h 
    imstb_textedit.h
    imstb_truetype.h 
    imgui_draw.cpp
    imgui_tables.cpp
    imgui_widgets.cpp
    imgui_demo.cpp
    # Adiciona APENAS os backends que o projeto realmente usa
    backends/imgui_impl_glfw.h backends/imgui_impl_glfw.cpp
    backends/imgui_impl_opengl3.h backends/imgui_impl_opengl3.cpp
)

add_library(ImGui STATIC ${IMGUI_SOURCES})

target_compile_features(ImGui PUBLIC cxx_std_17)

target_include_directories(ImGui PUBLIC ${CMAKE_CURRENT_SOURCE_DIR})

set_target_properties(ImGui PROPERTIES POSITION_INDEPENDENT_CODE ON)

if(MSVC)
    set_property(TARGET ImGUI PROPERTY MSVC_RUNTIME_LIBRARY "MultiThreaded$<$<CONFIG:Debug>:Debug>")
endif()
