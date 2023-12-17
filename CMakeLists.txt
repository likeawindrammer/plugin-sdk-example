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
  GIT_TAG a2ee4d8eb721907b8deb89093f5b740d4b8b56a0
)

add_library(ColouredObjects src/main.cpp)
set_target_properties(ColouredObjects PROPERTIES PREFIX "")
set_target_properties(ColouredObjects PROPERTIES SUFFIX ".asi")
target_compile_features(ColouredObjects PRIVATE cxx_std_20)
IF(MINGW)
	target_link_options(ColouredObjects PRIVATE -static-libgcc -static-libstdc++ -Wl,-Bstatic,--whole-archive -lwinpthread -Wl,--no-whole-archive -Wl,-Bdynamic)
ENDIF(MINGW)
target_link_libraries(ColouredObjects gtasa)
