cmake_minimum_required(VERSION 2.8)

project(async_client)

set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${CMAKE_SOURCE_DIR}/cmake/Modules/")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -pthread -std=c++14")

if (CMAKE_BUILD_TYPE STREQUAL Debug)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -D_DEBUG")
    #add_definitions(
    #    -D_DEBUG
    #)
endif ()

add_definitions(-DFMT_HEADER_ONLY)

find_package(Boost COMPONENTS coroutine context system thread chrono REQUIRED)

set(REST_RPC_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/../../)

include_directories(
		${Boost_INCLUDE_DIRS}
		${REST_RPC_DIRECTORY}
		${REST_RPC_DIRECTORY}/Kapok
		${REST_RPC_DIRECTORY}/Kapok/fmt
		${REST_RPC_DIRECTORY}/spdlog/include
		${REST_RPC_DIRECTORY}/msgpack/include
        ${REST_RPC_DIRECTORY}/thread-pool-cpp/thread_pool
		)
SET(EXTRA_LIBS ${EXTRA_LIBS} ${Boost_LIBRARIES} ${CMAKE_THREAD_LIBS_INIT})


set(SOURCE_FILES 
	async_client_example.cpp
	)


add_executable(async_client ${SOURCE_FILES})
target_link_libraries(async_client ${EXTRA_LIBS})

include (InstallRequiredSystemLibraries)
set (CPACK_PACKAGE_VERSION_MAJOR "1")
set (CPACK_PACKAGE_VERSION_MINOR "0")
SET(CPACK_DEBIAN_PACKAGE_MAINTAINER "David Doria")
include (CPack)
