
cmake_minimum_required(VERSION 3.4.1)

if( NOT IS_DIRECTORY ${BX_DIR} )
	message( SEND_ERROR "${BX_DIR}. Could not load bgfx, directory does not exist. " )
	return()
endif()

# 设置c++编译器的编译选项：使用gnu++11库，打开警告开关，将所有警告当成错误处理
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=gnu++11 -Wall -Werror")

# Grab the bx source files
file( GLOB BX_SOURCES ${BX_DIR}/src/*.cpp )
set_source_files_properties( ${BX_DIR}/src/amalgamated.cpp PROPERTIES HEADER_FILE_ONLY ON )

# Create the bx target
add_library( bx STATIC ${BX_SOURCES} )

# Add include directory of bx
target_include_directories( bx
	PUBLIC
        $<BUILD_INTERFACE:${BX_DIR}/include>
        $<BUILD_INTERFACE:${BX_DIR}/3rdparty>)

