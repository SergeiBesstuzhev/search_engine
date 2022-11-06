cmake_minimum_required(VERSION 3.23)
project(search_engine)

set(CMAKE_CXX_STANDARD 17)

set(JSON_BuildTests OFF CACHE INTERNAL "")

add_subdirectory(nlohmann_json)

add_executable(search_engine
        main.cpp
        src/ConverterJSON.cpp
        src/InvertedIndex.cpp
        src/SearchServer.cpp)

include_directories(${PROJECT_NAME} PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}/include)

target_link_libraries(search_engine PRIVATE nlohmann_json::nlohmann_json)

# Tests: =======================================================================================
project(Google_tests)
enable_testing()
include_directories(${PROJECT_NAME} PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}/include)
include_directories(${gtest_SOURCE_DIR}/include ${gtest_SOURCE_DIR})
add_subdirectory(${PROJECT_NAME}/lib)
add_executable(
        ${PROJECT_NAME}
        Google_tests/InverterIndexTest.cpp
        Google_tests/SearchServerTest.cpp
        src/InvertedIndex.cpp
        src/SearchServer.cpp
)
set_target_properties(${PROJECT_NAME} PROPERTIES
        CXX_STANDARD 17
        CXX_STANDARD_REQUIRED ON
        )
target_link_libraries(${PROJECT_NAME} gtest gtest_main)