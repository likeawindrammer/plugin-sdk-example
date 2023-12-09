cmake_minimum_required(VERSION 3.16)

project(plugin-example LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

set(CMAKE_INTERPROCEDURAL_OPTIMIZATION TRUE)

include(cmake/CPM.cmake)
set(CPM_SOURCE_CACHE cache)

CPMAddPackage(
  NAME plugin-sdk
  GITHUB_REPOSITORY likeawindrammer/plugin-sdk-mingw
  GIT_TAG 1d072020ccabebf58789b14fe089208733b9d979
)

add_library(ColouredObjects src/main.cpp)
set_target_properties(ColouredObjects PROPERTIES PREFIX "")
set_target_properties(ColouredObjects PROPERTIES SUFFIX ".asi")
target_compile_features(ColouredObjects PRIVATE cxx_std_20)
IF(MINGW)
	target_link_options(ColouredObjects PRIVATE -static-libgcc -static-libstdc++ -Wl,-Bstatic,--whole-archive -lwinpthread -Wl,--no-whole-archive -Wl,-Bdynamic)
ENDIF(MINGW)
target_link_libraries(ColouredObjects gtasa)
