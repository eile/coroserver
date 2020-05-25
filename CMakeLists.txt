cmake_minimum_required(VERSION 2.8)
project(coroserver)

set(CMAKE_CXX_FLAGS "${CMAKE_C_FLAGS} -std=c++11 -ftemplate-depth=256")
if(APPLE)
  set(CMAKE_LINK_FLAGS "${CMAKE_LINK_FLAGS} -stdlib=libc++")
endif(APPLE)

find_package(Boost 1.54.0
             COMPONENTS system
                        thread
                        coroutine
                        context
             REQUIRED)

include_directories(${Boost_INCLUDE_DIRS})

set(SOURCES
    calculator.cpp
    http_protocol.cpp
    routing.cpp
    server.cpp
    http-parser/http_parser.c)
add_library(coro ${SOURCES})
add_executable(coroserver main.cpp)
target_link_libraries(coroserver
                      coro
                      ${Boost_LIBRARIES}
                      pthread)
