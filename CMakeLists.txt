cmake_minimum_required(VERSION 3.10)

# download glew
include(./3rdparty/DownloadProject/DownloadProject.cmake)
download_project(
	PROJ glew_src
	URL https://github.com/nigels-com/glew/releases/download/glew-2.1.0/glew-2.1.0.tgz
)

# create project
project(opengl_template)

# configuration
set(CMAKE_CXX_STANDARD 14)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# add libraries subdirectory
set(LIBRARIES_PATH "${CMAKE_SOURCE_DIR}/3rdparty")
add_subdirectory(${glew_src_SOURCE_DIR}/build/cmake ${glew_src_BINARY_DIR} EXCLUDE_FROM_ALL)
add_subdirectory(${LIBRARIES_PATH}/glfw)
add_subdirectory(${LIBRARIES_PATH}/glm)
add_subdirectory(${LIBRARIES_PATH}/openal)

# add source files
file(GLOB_RECURSE ALL_CPP_FILES "${CMAKE_SOURCE_DIR}/src/*.cpp")
add_executable(opengl_template
	"${ALL_CPP_FILES}"
	"${LIBRARIES_PATH}/openal/examples/common/alhelpers.c"
)

# set include directories
target_include_directories(opengl_template PRIVATE
	"${glew_src_SOURCE_DIR}/include"
	"${LIBRARIES_PATH}/openal/examples"
)

# set link libraries
target_link_libraries(opengl_template
	glew
	glfw
	glm
	OpenAL
)
