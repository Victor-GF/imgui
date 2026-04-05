cmake_minimum_required(VERSION 3.16)
project(ImGUI)

file(GLOB_RECURSE IMGUI_SOURCES
        ./*.cpp ./*.h
)

add_library(ImGUI STATIC ${IMGUI_SOURCES})

target_compile_features(ImGUI PUBLIC cxx_std_17)

target_include_directories(ImGUI PUBLIC ${CMAKE_CURRENT_SOURCE_DIR})

set_target_properties(ImGUI PROPERTIES POSITION_INDEPENDENT_CODE ON)

if(MSVC)
    set_property(TARGET ImGUI PROPERTY MSVC_RUNTIME_LIBRARY "MultiThreaded$<$<CONFIG:Debug>:Debug>")
endif()
