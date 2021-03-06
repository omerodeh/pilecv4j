cmake_minimum_required (VERSION 3.4)
project (ai.kognition.pilecv4j.gstreamer)

find_package(PkgConfig)

set (CMAKE_CXX_STANDARD 11)

pkg_check_modules(GST_VIDEO gstreamer-video-1.0 REQUIRED)

if(NOT DEFINED ENV{COMMON_INCLUDE})
  message(FATAL_ERROR "You need to define the environment variable COMMON_INCLUDE to point to '[pilecv4j-root]/native-common-include/include' or build from the pom file which sets it correctly.")
endif()
include_directories($ENV{COMMON_INCLUDE})

link_directories(${GST_VIDEO_LIBRARY_DIRS})

add_library(gstbreakout SHARED
  gstbreakout.c
  gstmat.cpp
  gstbreakout-marshal.c
  )

target_include_directories(gstbreakout PRIVATE src/main/c PUBLIC ${GST_VIDEO_INCLUDE_DIRS})
target_compile_options(gstbreakout PUBLIC ${GST_VIDEO_CFLAGS_OTHER})
target_link_libraries(gstbreakout ${GST_VIDEO_LIBRARIES})

