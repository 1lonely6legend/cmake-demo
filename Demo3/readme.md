# 3-多个目录与源文件

## aux_source_directory()

`aux_source_directory()`函数用于将指定目录中的所有源文件（不包括头文件）添加到一个变量中。它的语法如下：

```cmake
aux_source_directory(dir VARIABLE)
```

其中，`dir`是源文件目录的路径，`VARIABLE`是用于存储源文件名列表的变量名称。

`aux_source_directory()`函数会将指定目录中的所有源文件添加到一个变量中，并将变量传递给后续的构建指令。这个函数通常用于简化CMake的构建脚本，因为您可以使用一个命令将所有源文件添加到构建目标中，而不必手动列出每个文件。

例如，假设您有一个名为`my_project`的项目，其中包含一个名为`my_library`的库，该库包含一些源文件。

您可以在my_library库目录下的CMakeLists.txt中添加以下代码将这些源文件添加到库中：

```cmake
# CMakeLists.txt in the my_library directory
aux_source_directory(. SRC_LIST)

add_library(my_library ${SRC_LIST})
```

在上面的示例中，`aux_source_directory()`函数将当前目录中的所有源文件添加到一个名为`SRC_LIST`的变量中。然后，该变量将传递给`add_library()`函数，以便将这些源文件作为参数添加到库中。

需要注意的是，`aux_source_directory()`函数只会将指定目录中的源文件添加到变量中，不会将该目录中的头文件添加到变量中。如果您想要将头文件也添加到变量中，可以使用`file(GLOB ...)`命令来实现。

## add_subdirectory()

`add_subdirectory()`函数用于向CMake构建系统添加一个子目录，并将其包含在整个项目的构建中。它的语法如下：

```cmake
add_subdirectory(source_dir [binary_dir] [EXCLUDE_FROM_ALL])
```

其中，`source_dir`是子目录的路径，`binary_dir`是用于指定构建输出目录的可选参数，默认为`${CMAKE_CURRENT_BINARY_DIR}/${source_dir}`。

`EXCLUDE_FROM_ALL`是可选参数，如果设置为`TRUE`，则子目录不会被包含在生成的all目标中。

`add_subdirectory()`函数**通常在项目根目录的CMakeLists.txt文件中使用**，用于**添加子目录并将子目录中的CMakeLists.txt文件包含到项目中**。

这允许您将项目分解为多个子目录和模块，并且使CMake的构建系统更加灵活和可维护。

例如，假设您有一个名为`my_project`的项目，其中包含一个名为`my_library`的子目录，该子目录包含一个CMakeLists.txt文件和一些源代码文件。

您可以使用以下代码将该子目录添加到项目中：

```cmake
# CMakeLists.txt in the root directory of my_project
cmake_minimum_required(VERSION 3.0)

project(my_project)

add_subdirectory(my_library)
```

在`my_library`子目录中，您可以有一个CMakeLists.txt文件，其中包含了构建my_library库的指令，例如：

```cmake
# CMakeLists.txt in the my_library directory
add_library(my_library source1.cpp source2.cpp)
```

通过这种方式，my_library库将与my_project的其他目标一起构建，并且可以在my_project的其他目标中使用。

## target_link_libraries()

`target_link_libraries()`函数用于将一个或多个库链接到一个目标文件中。它的语法如下：

```cmake
target_link_libraries(target library1 library2 ...)
```

其中，`target`是您要链接的目标文件的名称，`library1`、`library2`等是要链接到目标文件中的库的名称。

在CMake中，库可以是静态库或动态库。如果要链接一个静态库，只需指定库的名称即可；如果要链接一个动态库，可以指定库的名称和库的路径。通常，库的路径可以通过`link_directories()`指令设置，而库的名称可以通过`find_library()`函数或`target_link_libraries()`函数指定。

例如，假设您有一个名为`my_project`的项目，其中包含一个可执行文件`my_executable`，该可执行文件依赖于一个名为`my_library`的库。您可以使用以下代码将这些库链接到可执行文件中：

```cmake
# CMakeLists.txt in the my_project directory
add_subdirectory(my_library)

add_executable(my_executable main.cpp)

target_link_libraries(my_executable my_library)
```

在上面的示例中，`add_subdirectory()`函数会将`my_library`子目录中的库添加到项目中。然后，`add_executable()`函数会创建一个名为`my_executable`的可执行文件。最后，`target_link_libraries()`函数将`my_library`库链接到`my_executable`可执行文件中。

需要注意的是，`target_link_libraries()`函数还可以链接其他第三方库，例如系统库或第三方库。例如，您可以链接一个名为`m`的系统数学库，如下所示：

```cmake
target_link_libraries(my_executable m)
```

在这种情况下，`target_link_libraries()`函数将`m`库链接到`my_executable`可执行文件中。

## add_library ()

`add_library()`函数用于创建一个库文件，它的语法如下：

```cmake
add_library(<name> [STATIC | SHARED | MODULE]
            [EXCLUDE_FROM_ALL]
            [source1] [source2] ... [sourceN])
```

其中，`<name>`是库的名称，`STATIC`、`SHARED`和`MODULE`用于指定库的类型（可选，默认为静态库），`EXCLUDE_FROM_ALL`用于指定是否将库添加到ALL编译选项（可选），`source1`、`source2`等是库的源文件。

在CMake中，库可以是静态库、共享库或模块库。静态库是在链接时与可执行文件一起链接的库，共享库是在运行时与可执行文件一起加载的库，而模块库是在运行时动态加载的库。

例如，假设您有一个名为`my_library`的库，其中包含两个源文件`foo.cpp`和`bar.cpp`，您可以使用以下代码来创建这个库：

```cmake
# CMakeLists.txt in the my_library directory
aux_source_directory(. DIR_LIB_SRCS)

add_library(my_library STATIC ${DIR_LIB_SRCS})
```

在上面的示例中，`aux_source_directory()`函数会自动查找当前目录中的所有源文件，并将它们添加到`DIR_LIB_SRCS`变量中。然后，`add_library()`函数将`my_library`静态库创建为包含`DIR_LIB_SRCS`中的所有源文件的库。

需要注意的是，库可以在其他目标文件中使用。例如，您可以在一个可执行文件中链接`my_library`库，如下所示：

```cmake
# CMakeLists.txt in the my_project directory
add_subdirectory(my_library)

add_executable(my_executable main.cpp)

target_link_libraries(my_executable my_library)
```

在这种情况下，`target_link_libraries()`函数将`my_library`库链接到`my_executable`可执行文件中。

