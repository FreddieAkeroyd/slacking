cmake_minimum_required(VERSION 2.8)

project(examples)

find_package(CURL)
include_directories(${CURL_INCLUDE_DIRS})
include_directories(".")

set (SOURCES_EXAMPLES 
    00-showcase.cpp
    01-basic.cpp
    02-basic.cpp
    03-attachments.cpp
    04-users.cpp
    05-channels.cpp
    06-custom_post_get.cpp
)

set (TARGETS_EXAMPLES
    00-showcase
    01-basic
    02-basic
    03-attachments
    04-users
    05-channels
    06-custom_post_get
)

foreach( name ${TARGETS_EXAMPLES} )
    add_executable(${name} ${name}.cpp)
    set_property(TARGET ${name} PROPERTY CXX_STANDARD 11)
    set_property(TARGET ${name} PROPERTY CXX_STANDARD_REQUIRED ON)
    if(NOT WIN32)
        target_link_libraries(${name} ${CURL_LIBRARIES})
    else()
        link_directories(${CMAKE_BINARY_DIR})
        target_link_libraries(${name} curl) # for appveyor CI
    endif()
endforeach()

if(NOT WIN32)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall")
endif()

if ("${CMAKE_MAJOR_VERSION}${CMAKE_MINOR_VERSION}" LESS 31)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")
endif()

set(CMAKE_CXX_FLAGS_DEBUG "${CMAKE_CXX_FLAGS_DEBUG}")

add_definitions(-DSLACKING_VERBOSE_OUTPUT=1)
