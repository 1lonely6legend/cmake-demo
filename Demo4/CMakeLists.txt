# CMake 最低版本号要求
cmake_minimum_required (VERSION 2.8)

# 项目信息
project (Demo4)

#set用设置普通变量的值,这里设置了一个变量CMAKE_INCLUDE_CURRENT_DIR = ON
#[[作用是将当前源码目录添加到包含文件搜索路径中。在 CMake 构建时，如果使用了该指令，CMake 会在构建目录下生成一个包含了当前源码目录的
_build 目录。然后在该目录下执行构建命令时，会将当前源码目录添加到编译器的包含路径中。这样就可以方便地包含当前源码目录下的头文件，
而不需要在代码中使用绝对路径或相对路径来引用头文件。]]
set(CMAKE_INCLUDE_CURRENT_DIR ON)

# 是否使用自己的 MathFunctions 库
# 相当于camke的语法了,设置一个变量
# option相当于定义一个cmake选项,来开启或者关闭某个功能
option (USE_MYMATH
	   "Use provided math implementation" OFF)

# 加入一个配置头文件，用于处理 CMake 对源码的设置
# 会自动生成一个config.h文件
# 可以通过h.in文件来设置一些变量,控制代码的编译
# cmake根据h.in文件中的内容生成一个h文件
configure_file (
  "${PROJECT_SOURCE_DIR}/config.h.in"
  "${PROJECT_BINARY_DIR}/config.h"
  )

# 是否加入 MathFunctions 库
if(USE_MYMATH)
    # 将math添加到include路径中
    # 这样cmake在编译过程中就能直接找到math中的头文件
    # 编写main的时候就不需要include相对路径了
    include_directories("${PROJECT_SOURCE_DIR}/math")
    add_subdirectory(math)
    # 将 EXTRA_LIBS 的值与字符串 "MathFunctions" 连接，重新赋值给 EXTRA_LIBS
    set (EXTRA_LIBS ${EXTRA_LIBS} MathFunctions)
else()
    # 不链接math库会报错，因为linux中默认没有math库
    LINK_LIBRARIES(m)
endif(USE_MYMATH)

# 查找当前目录下的所有源文件
# 并将名称保存到 DIR_SRCS 变量
aux_source_directory(. DIR_SRCS)

# 指定生成目标
add_executable (Demo ${DIR_SRCS})
target_link_libraries (Demo  ${EXTRA_LIBS})
