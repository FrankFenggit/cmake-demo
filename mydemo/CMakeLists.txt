# CMake 最低版本号要求
cmake_minimum_required (VERSION 2.8)

# 项目信息
project (mainexe)
message(STATUS "CMAKE_PROJECT_NAME = ${CMAKE_PROJECT_NAME}")

#编译器参数
set(COMPILER_BIN /usr/bin )
set(CMAKE_CXX_COMPILER ${COMPILER_BIN}/g++ CACHE STRING "C++ compiler" FORCE)
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")

# 设置输出目录
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)

# 头文件
include_directories("${PROJECT_SOURCE_DIR}/")

# 添加需要链接的库文件目录
LINK_DIRECTORIES("${CMAKE_LIBRARY_OUTPUT_DIRECTORY}")

# 添加子模块
add_subdirectory(math)
add_subdirectory(main)

# 指定生成库文件（动态库、静态库 二选其一，也可以都要）  
add_library(math_lib_shared  SHARED $<TARGET_OBJECTS:MathFunctionsOBJ>) #动态库
add_library(math_lib_static  STATIC $<TARGET_OBJECTS:MathFunctionsOBJ>) #静态库
set_target_properties(math_lib_shared PROPERTIES OUTPUT_NAME "mymath")
set_target_properties(math_lib_static PROPERTIES OUTPUT_NAME "mymath")

# 指定生成目标 
add_executable(${CMAKE_PROJECT_NAME} $<TARGET_OBJECTS:MainOBJ>)
target_link_libraries(${CMAKE_PROJECT_NAME} mymath) # 添加链接库

# 指定构建依赖顺序
add_dependencies(${CMAKE_PROJECT_NAME} math_lib_static math_lib_shared) 
