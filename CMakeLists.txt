cmake_minimum_required (VERSION 2.8)
project (Simple-WebSocket-Server)
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++1y -O3 -Wno-deprecated-declarations")

if("${CMAKE_CXX_COMPILER_ID}" STREQUAL "GNU")
    if (CMAKE_CXX_COMPILER_VERSION VERSION_LESS 4.9)
        message(FATAL_ERROR "GCC version >=4.9 required.")
    endif()
elseif ("${CMAKE_CXX_COMPILER_ID}" STREQUAL "Clang")
    if (CMAKE_CXX_COMPILER_VERSION VERSION_LESS 3.4)
        message(FATAL_ERROR "Clang version >=3.4 required.")
    endif()
else()
    message(WARNING "Your compiler (${CMAKE_CXX_COMPILER_ID}) has not been tested on this project. Only Clang and GCC has been tested. Please report any problems at the project page on GitHub.")
endif()

#Only tested with versions 1.55 and 1.56
find_package(Boost 1.41.0 COMPONENTS system REQUIRED)
message("Boost include dir: ${Boost_INCLUDE_DIR}")
message("Boost libraries: ${Boost_LIBRARIES}")
include_directories(${Boost_INCLUDE_DIR})

#TODO: add requirement for version 1.0.1g (can it be done in one line?)
find_package(OpenSSL REQUIRED)
message("OpenSSL include dir: ${OPENSSL_INCLUDE_DIR}")
message("OpenSSL crypto libraries: ${OPENSSL_CRYPTO_LIBRARIES}")
message("OpenSSL SSL libraries: ${OPENSSL_SSL_LIBRARIES}")
include_directories(${OPENSSL_INCLUDE_DIR})

find_package(Threads REQUIRED)
message("Threads libraries/flag: ${CMAKE_THREAD_LIBS_INIT}")

include_directories(.)

add_executable(wss_examples wss_examples.cpp)
target_link_libraries(wss_examples ${Boost_LIBRARIES})
target_link_libraries(wss_examples ${OPENSSL_CRYPTO_LIBRARIES})
target_link_libraries(wss_examples ${OPENSSL_SSL_LIBRARIES})
target_link_libraries(wss_examples ${CMAKE_THREAD_LIBS_INIT})

add_executable(ws_examples ws_examples.cpp)
target_link_libraries(ws_examples ${Boost_LIBRARIES})
target_link_libraries(ws_examples ${OPENSSL_CRYPTO_LIBRARIES})
target_link_libraries(ws_examples ${CMAKE_THREAD_LIBS_INIT})

enable_testing()
add_subdirectory(test)
