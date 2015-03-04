cmake_minimum_required(VERSION 2.8)

project (GAFPlayer)

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

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -fno-exceptions -std=c++11 -Wno-deprecated-declarations -Wno-reorder")

if(IOS OR MACOX)
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -stdlib=libc++")
endif()

include_directories(
  /usr/include/
  /usr/local/include/
  /System/Library/Frameworks
  ${CMAKE_CURRENT_SOURCE_DIR}/Sources
  ${CMAKE_CURRENT_SOURCE_DIR}/Sources/ShadersPrecompiled
  ${CMAKE_CURRENT_SOURCE_DIR}
  ${CMAKE_CURRENT_SOURCE_DIR}/glfw/include
  ${CMAKE_CURRENT_SOURCE_DIR}/cocos
  ${CMAKE_CURRENT_SOURCE_DIR}/cocos/math
  ${CMAKE_CURRENT_SOURCE_DIR}/cocos/base
)


set(CMAKE_EXE_LINKER_FLAGS
    "-framework AudioToolbox -framework CoreGraphics -framework QuartzCore -framework UIKit -framework OpenGLES"
)

file(GLOB_RECURSE gaf_source_files
  "${CMAKE_CURRENT_SOURCE_DIR}/Sources/*.cpp")

add_library(gafplayer ${gaf_source_files})

#install .a file
INSTALL(
  TARGETS gafplayer
  ARCHIVE DESTINATION lib
  LIBRARY DESTINATION lib
  COMPONENT library
)

#install header files
install(DIRECTORY Sources/ DESTINATION "include/GAFPlayer"
  FILES_MATCHING PATTERN "*.h") 