# CMake 最低版本号要求
cmake_minimum_required(VERSION 2.8)

# 项目信息
project(Demo3)

# 查找目录下的所有源文件
# 并将名称保存到 DIR_SRCS 变量
aux_source_directory(. DIR_SRCS)

############################################
#add_subdirectory()函数相当于告诉CMake去哪里找CMakeLists.txt文件,多了一层目录
############################################

# 添加 math 子目录
# 子目录中还有对应的子目录的cmakelist.txt文件
add_subdirectory(math)
# CMake将在指定的目录中搜索CMakeLists.txt文件，并在该目录中执行该文件中的命令

# 指定生成目标
add_executable(Demo ${DIR_SRCS})

############################################
#target_link_libraries()函数用于将一个或多个库文件,不一定非要是.h文件,
#现在就是将math子目录中的MathFunctions库文件链接到Demo中
############################################

# 添加链接库
target_link_libraries(Demo MathFunctions)
# 该函数用于将一个或多个库文件,不一定非要是.h文件,
# 或目标文件链接到一个指定的目标文件或可执行文件中。