# CMake 最低版本号要求
cmake_minimum_required (VERSION 2.8)

# 项目信息
project (Demo2)

# 查找.目录下的所有.cc和.c源文件
# 并将名称保存到 DIR_SRCS 变量
# 第一个参数是要添加源文件的目录,第二个参数是存储所有源文件的变量名
aux_source_directory(. DIR_SRCS)
# 其中aux是auxiliary的缩写,意为辅助的意思
# aux通常用于表示辅助函数或模块，这些函数或模块不是CMake的核心功能，但它们可以帮助简化构建过程


# 指定生成目标
add_executable(Demo ${DIR_SRCS})
