cmake_minimum_required(VERSION 3.11)
project(main)

# Generate compile_commands.json
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

# Settings C/C++ standard
set(CMAKE_C_STANDARD 11)
set(CMAKE_CXX_STANDARD 11)

# find raylib
find_package(raylib CONFIG QUIET)

# You could change the QUIET above to REQUIRED and remove this if() clause
# This part downloads raylib and builds it if it's not installed on your system
if (NOT raylib_FOUND) # If there's none, fetch and build raylib
	include(FetchContent)

	FetchContent_Declare(
		raylib
		URL https://github.com/raysan5/raylib/archive/master.tar.gz
	)
	FetchContent_GetProperties(raylib)
	if (NOT raylib_POPULATED) # Have we downloaded raylib yet?
		set(FETCHCONTENT_QUIET NO)
		FetchContent_Populate(raylib)

		set(BUILD_EXAMPLES OFF CACHE BOOL "" FORCE) # don't build the supplied examples

		# build raylib
		add_subdirectory(${raylib_SOURCE_DIR} ${raylib_BINARY_DIR})

	endif()
endif()

# Include any headers and c files to add to the executable
include_directories(
	${PROJECT_SOURCE_DIR}/include
	${PROJECT_SOURCE_DIR}/src
)

file(GLOB main_sources
	"${PROJECT_SOURCE_DIR}/include/*.h"
	"${PROJECT_SOURCE_DIR}/include/*.hpp"
	"${PROJECT_SOURCE_DIR}/src/*.c"
	"${PROJECT_SOURCE_DIR}/src/*.cpp"
)

add_executable(${PROJECT_NAME} ${main_sources})

target_link_libraries(${PROJECT_NAME} raylib)

if (EMSCRIPTEN)
    set(CMAKE_EXE_LINKER_FLAGS "-s USE_GLFW=3")
endif ()

# Checks if OSX and links appropriate frameworks (Only required on MacOS)
if (APPLE)
	target_link_libraries(${PROJECT_NAME} "-framework IOKit")
	target_link_libraries(${PROJECT_NAME} "-framework Cocoa")
	target_link_libraries(${PROJECT_NAME} "-framework OpenGL")
endif()