cmake_minimum_required(VERSION 3.3)
# Some cutoms functions
include(./cmake/functions.txt)

# Library name
set(NAME RTClib)

# Files
file(GLOB_RECURSE ${NAME}_FILES_CPP "*.cpp")
file(GLOB_RECURSE ${NAME}_FILES_H "*.h")

# Ignore some files
include(./cmake/ignore.txt)

# ARDUINO MODE
if (ARDUINO_FOUND)
    # Declare arduino library
    generate_arduino_library(
        ${NAME}
        SRCS ${${NAME}_FILES_CPP}
        HDRS ${${NAME}_FILES_H}
        BOARD ${BOARD}
        ARDLIBS Wire
#        NO_AUTOLIBS
    )
# OTHER MODE (Raspberry)
else()
    # Define library
    project("lib${NAME}")
    add_library(${NAME} ${${NAME}_FILES_CPP} ${${NAME}_FILES_H})
endif()

# Compiler
set_property(TARGET ${NAME} PROPERTY CXX_STANDARD 11)
set_property(TARGET ${NAME} PROPERTY CXX_STANDARD_REQUIRED ON)

# Permit to include files (similar as defining ./ directory to include path)
target_include_directories(${NAME} PUBLIC .)
