#
# Copyright (C) The Android Open Source Project
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#      http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
#

cmake_minimum_required(VERSION 3.4.1)

# build native_app_glue as a static lib
# set(${CMAKE_C_FLAGS}, "${CMAKE_C_FLAGS}")

set_property( GLOBAL PROPERTY USE_FOLDERS ON )
set(CMAKE_CXX_STANDARD 14)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

# 设置c++编译器的编译选项：使用gnu++11库，打开警告开关，将所有警告当成错误处理
# set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=gnu++11 -Wall -Werror")

set( BGFX_OPENGL_VERSION   ""        CACHE STRING "Specify minimum opengl version" )
set( BGFX_OPENGLES_VERSION ""        CACHE STRING "Specify minimum OpenGL ES version" )
set( BGFX_LIBRARY_TYPE     "STATIC"     CACHE STRING "Linking type for library" )

# 添加bx库
set(BX_DIR)
get_filename_component(BX_DIR "${CMAKE_CURRENT_SOURCE_DIR}/../../../../../bx" ABSOLUTE)
add_subdirectory(${BX_DIR} bx)

# 添加bimg库
set(BIMG_DIR)
get_filename_component(BIMG_DIR "${CMAKE_CURRENT_SOURCE_DIR}/../../../../../bimg" ABSOLUTE)
add_subdirectory(${BIMG_DIR} bimg)

# 添加bimg-decode库
#set(BIMG-DECODE_DIR ../../../../../bimg/src)
#add_subdirectory(${BIMG-DECODE_DIR} bimg-decode)

# 添加ib-compress库
#set(ib-compress_dir ../../../../../bgfx/3rdparty/ib-compress)
#add_subdirectory(${ib-compress_dir} ib-compress)

# 添加bgfx库
set(BGFX_DIR)
get_filename_component(BGFX_DIR "${CMAKE_CURRENT_SOURCE_DIR}/../../../../../bgfx" ABSOLUTE)
add_subdirectory(${BGFX_DIR} bgfx)

# 添加dear-imgui库
#set(dear-imgui_dir)
#get_filename_component(dear-imgui_dir "${CMAKE_CURRENT_SOURCE_DIR}/../../../../../bgfx/3rdparty/dear-imgui" ABSOLUTE)
#add_subdirectory(${dear-imgui_dir} dear-imgui)

# 添加examples-common库
set(examples-common_dir)
get_filename_component(examples-common_dir "${CMAKE_CURRENT_SOURCE_DIR}/../../../../../bgfx/examples/common" ABSOLUTE)
add_subdirectory(${examples-common_dir} examples-common)

set(CMAKE_SHARED_LINKER_FLAGS
        "${CMAKE_SHARED_LINKER_FLAGS} -u ANativeActivity_onCreate")

# 编译成共享库
add_library(native-activity SHARED main.cpp)

# 设置目标库的包含目录
target_include_directories(native-activity PRIVATE
        ./
        ${ANDROID_NDK}/sources/android/native_app_glue
        ${BX_DIR}/include
        ${BIMG_DIR}/include
        ${BGFX_DIR}/include
        ${BGFX_DIR}/3rdparty
        ${examples-common_dir})

# 设置目标库的链接库
target_link_libraries(native-activity
        bgfx
        examples-common
        #dear-imgui
        )

