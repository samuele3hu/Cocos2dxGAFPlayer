# this CMakeLists is to generate gafluabindings.a/gafluabindings.lib

cmake_minimum_required(VERSION 2.8)

project (GAFLuaBindings)

if(IOS)
add_definitions(-DCC_TARGET_OS_IPHONE)
elseif(MACOX)
# set( mac)
# add_definitions(-DCC_TARGET_OS_MAC)
elseif(LINUX)
add_definitions(-DLINUX)
elseif(WIN32)
add_definitions(-D_WINDOWS)
elseif(ANDROID)
add_definitions(-DANDROID)
endif(IOS)

set(COCOS_ROOT ${CMAKE_CURRENT_SOURCE_DIR}/../Library/cocos)
set(GAF_ROOT ${CMAKE_CURRENT_SOURCE_DIR}/../Library)

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -fno-exceptions -std=c++11 -Wno-deprecated-declarations -Wno-reorder")

if(IOS OR MACOX)
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -stdlib=libc++")
endif()

include_directories(
  /usr/include/
  /usr/local/include/
  /System/Library/Frameworks
  ${GAF_ROOT}/Sources
  ${GAF_ROOT}/external/lua/tolua
  ${GAF_ROOT}/external/lua/lua
  ${COCOS_ROOT}/scripting/lua-bindings/manual
  ${COCOS_ROOT}
  ${COCOS_ROOT}/2d
  ${GAF_ROOT}/glfw/include
  ${GAF_ROOT}/cocos
  ${GAF_ROOT}/cocos/math
  ${GAF_ROOT}/cocos/base
)


set(CMAKE_EXE_LINKER_FLAGS
    "-framework AudioToolbox -framework CoreGraphics -framework QuartzCore -framework UIKit -framework OpenGLES"
)

file(GLOB_RECURSE gaf_binding_files
  "${CMAKE_SOURCE_DIR}/bindings/*.cpp")

file(GLOB_RECURSE gaf_source_files
  "${GAF_ROOT}/Sources/*.cpp")

add_library(gafluabindings ${gaf_binding_files} ${gaf_source_files})

#install .a file
INSTALL(
  TARGETS gafluabindings
  ARCHIVE DESTINATION lib
  LIBRARY DESTINATION lib
  COMPONENT library
)

#install header files
install(DIRECTORY ${GAF_ROOT}/Sources/ DESTINATION "include/GAFPlayerLua"
  FILES_MATCHING PATTERN "*.h") 

install(DIRECTORY bindings/ DESTINATION "include/GAFPlayerLua"
  FILES_MATCHING PATTERN "*.h") 
