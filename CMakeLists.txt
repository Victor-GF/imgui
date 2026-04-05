cmake_minimum_required(VERSION 3.16)
project(ImGui)

file(GLOB_RECURSE IMGUI_SOURCES
        ./*.cpp ./*.h
)

add_library(ImGui STATIC ${IMGUI_SOURCES})

target_compile_features(ImGui PUBLIC cxx_std_17)

target_include_directories(ImGui PUBLIC ${CMAKE_CURRENT_SOURCE_DIR})

set_target_properties(ImGui PROPERTIES POSITION_INDEPENDENT_CODE ON)

if(MSVC)
    set_property(TARGET ImGui PROPERTY MSVC_RUNTIME_LIBRARY "MultiThreaded$<$<CONFIG:Debug>:Debug>")
endif()
