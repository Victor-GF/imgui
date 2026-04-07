project(ImGui)

# 1. Lista de arquivos baseada na imagem do Premake
# Adicionei imgui_tables.cpp pois sem ele o docking não compila
file(GLOB_RECURSE IMGUI_SOURCES
    imgui.cpp
    imgui_draw.cpp
    imgui_widgets.cpp
    imgui_tables.cpp
    imgui_demo.cpp
    imgui.h
    imconfig.h
    imgui_internal.h
    imstb_rectpack.h
    imstb_textedit.h
    imstb_truetype.h
)

# 2. Cria a biblioteca estática
add_library(ImGui STATIC ${IMGUI_SOURCES})

# 3. Configurações de C++ (cppdialect "C++17")
target_compile_features(ImGui PUBLIC cxx_std_17)

# 4. Inclui o diretório atual para que os headers sejam encontrados
target_include_directories(ImGui PUBLIC ${CMAKE_CURRENT_SOURCE_DIR})

# 5. Configurações específicas de plataforma (Equivalente ao filter "system:windows")
if(WIN32)
    # No Windows, o Cherno usa Runtime Estático (/MT)
    set_target_properties(ImGui PROPERTIES
        MSVC_RUNTIME_LIBRARY "MultiThreaded$<$<CONFIG:Debug>:Debug>"
    )
    # Define macros úteis para Windows se necessário
    target_compile_definitions(ImGui PUBLIC _CRT_SECURE_NO_WARNINGS)
else()
    # No Linux, geralmente precisamos de fPIC para bibliotecas estáticas 
    # que serão usadas em shared libraries (.so) como a Hazel
    set_target_properties(ImGui PROPERTIES POSITION_INDEPENDENT_CODE ON)
endif()

# ---------------------------------------------------------
# IMPORTANTE: Adicione os arquivos de BACKEND (GLFW/OpenGL)
# O ImGui sozinho não desenha nada, ele precisa do código de "cola"
# ---------------------------------------------------------
# Se os arquivos estiverem na pasta backends/:
# target_sources(ImGui PRIVATE 
#    backends/imgui_impl_glfw.cpp
#    backends/imgui_impl_opengl3.cpp
# )
# target_include_directories(ImGui PUBLIC backends)