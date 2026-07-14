# ------------------------------------------------------------------
# Alterdune — CMake build (C++17 / SFML 3 / ImGui-SFML)
#
# SFML, Dear ImGui and ImGui-SFML are fetched automatically, so you
# don't need to install them by hand — just configure and build:
#
#   cmake -S . -B build
#   cmake --build build --config Release
#
# The executable is produced in build/ (or build/Release/ on MSVC),
# with the Ressources/ folder and the required DLLs copied next to it.
#
# NOTE: the version tags below (SFML 3.0.2, ImGui-SFML v3.0, ImGui
# v1.91.5) are known to work together. If a build ever breaks after an
# upstream change, pin these tags to the last compatible releases.
# ------------------------------------------------------------------

cmake_minimum_required(VERSION 3.21)
project(Alterdune LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

include(FetchContent)

# --- SFML 3 (window, graphics, audio, system) ---
FetchContent_Declare(SFML
    GIT_REPOSITORY https://github.com/SFML/SFML.git
    GIT_TAG        3.0.2
    GIT_SHALLOW    ON)
FetchContent_MakeAvailable(SFML)

# --- Dear ImGui (immediate-mode GUI) ---
FetchContent_Declare(imgui
    GIT_REPOSITORY https://github.com/ocornut/imgui.git
    GIT_TAG        v1.91.5
    GIT_SHALLOW    ON)
FetchContent_MakeAvailable(imgui)

# --- ImGui-SFML (binding between ImGui and SFML 3) ---
set(IMGUI_DIR ${imgui_SOURCE_DIR})
set(IMGUI_SFML_FIND_SFML OFF)    # reuse the SFML fetched above
set(IMGUI_SFML_IMGUI_DEMO OFF)
FetchContent_Declare(imgui-sfml
    GIT_REPOSITORY https://github.com/SFML/imgui-sfml.git
    GIT_TAG        v3.0
    GIT_SHALLOW    ON)
FetchContent_MakeAvailable(imgui-sfml)

# --- The Alterdune executable ---
add_executable(Alterdune
    src/main.cpp
    src/game.cpp
    src/interface.cpp
    src/entity.cpp
    src/player.cpp
    src/monster.cpp
    src/action.cpp
    src/item.cpp
    src/inventory.cpp
)

target_include_directories(Alterdune PRIVATE src)

target_link_libraries(Alterdune PRIVATE
    SFML::Graphics
    SFML::Window
    SFML::System
    SFML::Audio
    ImGui-SFML::ImGui-SFML
)

# Copy the game data (Ressources/CSV, fonts, images, audio) next to the
# executable, and make sure an empty Save/ folder exists so the game can
# write its save file.
add_custom_command(TARGET Alterdune POST_BUILD
    COMMAND ${CMAKE_COMMAND} -E copy_directory
        ${CMAKE_SOURCE_DIR}/Ressources $<TARGET_FILE_DIR:Alterdune>/Ressources
    COMMAND ${CMAKE_COMMAND} -E make_directory
        $<TARGET_FILE_DIR:Alterdune>/Save
)

# On Windows, copy the runtime DLLs (SFML and its dependencies) next to
# the executable so it runs from anywhere.
if(WIN32)
    add_custom_command(TARGET Alterdune POST_BUILD
        COMMAND ${CMAKE_COMMAND} -E copy_if_different
            $<TARGET_RUNTIME_DLLS:Alterdune> $<TARGET_FILE_DIR:Alterdune>
        COMMAND_EXPAND_LISTS)
endif()
