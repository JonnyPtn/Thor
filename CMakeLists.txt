#################################################################################
##
## Thor C++ Library
## Copyright (c) 2011-2022 Jan Haller
##
## This software is provided 'as-is', without any express or implied
## warranty. In no event will the authors be held liable for any damages
## arising from the use of this software.
##
## Permission is granted to anyone to use this software for any purpose,
## including commercial applications, and to alter it and redistribute it
## freely, subject to the following restrictions:
##
## 1. The origin of this software must not be misrepresented; you must not
##    claim that you wrote the original software. If you use this software
##    in a product, an acknowledgment in the product documentation would be
##    appreciated but is not required.
##
## 2. Altered source versions must be plainly marked as such, and must not be
##    misrepresented as being the original software.
##
## 3. This notice may not be removed or altered from any source distribution.
##
#################################################################################

# Directory Thor


cmake_minimum_required(VERSION 3.29)

project(thor)

# Predefined configuration options
option(THOR_BUILD_EXAMPLES "Build example projects" FALSE)

# Fetch SFML
include(FetchContent)
FetchContent_Declare(
	sfml
	GIT_REPOSITORY https://github.com/SFML/SFML
	GIT_TAG 2.6.2
)
FetchContent_MakeAvailable(sfml)

# Enable IDE folders
set_property(GLOBAL PROPERTY USE_FOLDERS ON)

# C++ source code
file(GLOB SRC CONFIGURE_DEPENDS src/*.cpp)

# The main library
set(THOR_LIB ${PROJECT_NAME})
add_library(${THOR_LIB} ${SRC})

# It's include paths
target_include_directories(${THOR_LIB} PRIVATE include extlibs/aurora/include)

# Link it to SFML
target_link_libraries(${THOR_LIB} PRIVATE sfml-graphics)

# Definition required for dll exports
target_compile_definitions(${THOR_LIB} PRIVATE THOR_EXPORTS)

# Set IDE folder for main project
set_target_properties(${PROJECT_NAME} PROPERTIES FOLDER "Thor")

if(THOR_BUILD_EXAMPLES)
	add_subdirectory(examples)
endif()
