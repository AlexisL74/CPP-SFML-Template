cmake_minimum_required(VERSION 3.29)  # 3.30 n'est pas encore une version stable, 3.29 est la dernière validée

project(ProjectAlbireo VERSION 0.0.1)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# SFML setup
include(FetchContent)
FetchContent_Declare(
        SFML
        GIT_REPOSITORY https://github.com/SFML/SFML.git
        GIT_TAG 3.0.0
)
FetchContent_MakeAvailable(SFML)

# Déclaration de l'exécutable
add_executable(ProjectAlbireo src/main.cpp
        config.h.in)

# Lier SFML à l'exécutable
target_link_libraries(ProjectAlbireo SFML::System SFML::Window SFML::Graphics SFML::Audio)

# Copy assets to build directory
add_custom_command(TARGET ProjectAlbireo POST_BUILD
        COMMAND ${CMAKE_COMMAND} -E copy_directory
        ${CMAKE_SOURCE_DIR}/assets $<TARGET_FILE_DIR:ProjectAlbireo>/assets
)

# Include le dossier de compilation pour config.h
include_directories(${CMAKE_BINARY_DIR})

# Versioning
message(STATUS "Project version: ${PROJECT_VERSION}")

configure_file(
        ${CMAKE_SOURCE_DIR}/config.h.in
        ${CMAKE_BINARY_DIR}/config.h
)
