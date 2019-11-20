cmake_minimum_required(VERSION 2.8)
project(coroserver)

set(CMAKE_CXX_FLAGS "${CMAKE_C_FLAGS} -std=c++11 -ftemplate-depth=256")
if(APPLE)
    set(CMAKE_LINK_FLAGS "${CMAKE_LINK_FLAGS} -stdlib=libc++")
endif(APPLE)

find_package(Boost 1.54.0 COMPONENTS system thread coroutine context REQUIRED)

find_package(PkgConfig REQUIRED)
pkg_check_modules(libturbojpeg libturbojpeg REQUIRED)

include_directories(${Boost_INCLUDE_DIRS} ${libturbojpeg_INCLUDE_DIRS})
add_definitions(-DBOOST_COROUTINES_NO_DEPRECATION_WARNING)

add_subdirectory(basis_universal)

file(GLOB SOURCE_LIST "*.cpp" "http-parser/http_parser.c")

add_executable(coroserver ${SOURCE_LIST})
target_link_libraries(coroserver
                      ${Boost_LIBRARIES}
                      ${libturbojpeg_LIBRARIES}
                      pthread
                      basisuniversal)
