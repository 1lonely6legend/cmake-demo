# 4-修改cmakelist文件

## set()

在CMake中，`set()`函数**用于设置变量的值**。它的语法如下：

```cmake
set(<variable> <value>... [PARENT_SCOPE])
```

其中，`<variable>`是变量的名称，`<value>`是变量的值，可以有多个，`[PARENT_SCOPE]`表示将变量作为全局变量进行设置。

例如，以下代码将设置一个名为`MY_VARIABLE`的变量，并将其值设置为`"Hello, world!"`：

```cmake
set(MY_VARIABLE "Hello, world!")
```

您还可以将变量设置为其他变量的值：

```cmake
set(MY_VARIABLE "Hello")
set(YOUR_VARIABLE "World")
set(OUR_VARIABLE "${MY_VARIABLE}, ${YOUR_VARIABLE}!")
```

在上面的示例中，`OUR_VARIABLE`的值将设置为`"Hello, World!"`。

此外，`set()`函数还**可以用于设置其他CMake选项**，如：

- `CMAKE_BUILD_TYPE`：指定构建类型，例如`Debug`或`Release`。
- `CMAKE_CXX_STANDARD`：指定C++标准，例如`11`或`14`。
- `CMAKE_INSTALL_PREFIX`：指定安装目录。
- 等等。

总之，`set()`函数是CMake中非常重要的函数之一，可以在构建过程中设置各种变量和选项。

## option()

`option()`函数用于定义一个CMake变量，该变量控制是否启用特定功能。它的语法如下：

```cmake
option(<variable> "help string" [initial value])
```

其中，`<variable>`是变量名称，`"help string"`是用于描述变量用途的字符串，`[initial value]`是变量的初始值（可选）。

在您提供的示例中，`option()`函数定义了一个名为`USE_MYMATH`的变量，并提供了一个帮助字符串，该变量控制是否使用提供的数学实现。初始值为ON，表示默认情况下启用该选项。

在CMakeLists.txt文件中，您可以使用`if()`函数根据`USE_MYMATH`变量的值来选择是否编译和链接数学实现。例如：

```cmake
if (USE_MYMATH)
  add_library(MathFunctions math/MathFunctions.cc)
  target_include_directories(MathFunctions PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}/math)
endif()
```

在这个例子中，如果`USE_MYMATH`变量的值为ON，则使用`add_library()`函数创建一个名为`MathFunctions`的库，并将`math/MathFunctions.cc`文件添加到该库中。

然后使用`target_include_directories()`函数将`math`目录添加到该库的include路径中，以便在编译期间能够找到MathFunctions.h头文件。

如果`USE_MYMATH`变量的值为OFF，则不会编译和链接数学实现，并且不会添加`math`目录到include路径中。

## option()和set()的区别

`option()` 和 `set()` 都是 CMake 中用于设置变量的函数，但它们的作用和用法是不同的。

`option()` 用于定义一个 CMake 选项，例如开启或关闭某些特性。它的语法如下：

```
option(<option_variable> "<help_string>" [initial_value])
```

其中，`<option_variable>` 是选项变量的名称，`"<help_string>"` 是选项的描述文本，`[initial_value]` 是选项的初始值，默认为 `OFF`。当用户在 CMake GUI 或命令行中选择了该选项时，选项变量的值将被设置为 `ON`，否则将被设置为 `OFF`。

例如，以下代码定义了一个名为 `USE_MY_LIBRARY` 的选项，并将其描述为 "Use my library"：

```
option(USE_MY_LIBRARY "Use my library" ON)
```

在 CMakeLists.txt 中使用该选项时，可以这样做：

```
if(USE_MY_LIBRARY)
    add_library(my_library ...)
    target_link_libraries(my_target my_library)
endif()
```

`set()` 函数则用于设置普通变量的值，例如源代码目录、二进制目录、安装目录等。它的语法如下：

```
set(<variable> <value>... [PARENT_SCOPE])
```

其中，`<variable>` 是变量的名称，`<value>` 是变量的值，可以有多个，`[PARENT_SCOPE]` 表示将变量作为全局变量进行设置。

例如，以下代码将设置一个名为 `MY_VARIABLE` 的变量，并将其值设置为 `"Hello, world!"`：

```
set(MY_VARIABLE "Hello, world!")
```

总之，`option()` 和 `set()` 都是非常常用的 CMake 函数，它们用于不同的场景。`option()` 用于定义选项，方便用户在构建过程中进行选择，而 `set()` 则用于设置各种变量和选项。

## configure_file()

`configure_file()`函数用于**将指定的源文件复制到指定的目标位置，并替换其中的变量值**。它的语法如下：

```cmake
configure_file(<input> <output>
               [COPYONLY] [ESCAPE_QUOTES] [@ONLY]
               [NEWLINE_STYLE [UNIX|DOS|WIN32|LF|CRLF]])
```

其中，`<input>`是源文件的名称，`<output>`是目标文件的名称，`COPYONLY`表示只复制文件而不进行变量替换，`ESCAPE_QUOTES`表示在替换变量时将引号进行转义，`@ONLY`表示只替换`@`符号而不替换`${}`符号，`NEWLINE_STYLE`表示指定输出文件的换行符风格。

`configure_file()`函数**主要用于在生成代码时替换变量值**。例如，您可以使用以下代码来替换源文件中的变量：

```cmake
# CMakeLists.txt in the my_project directory
set(MY_VARIABLE "Hello, world!")
configure_file(config.h.in config.h)

add_executable(my_executable main.cpp)
```

在上面的示例中，`config.h.in`文件中包含一个变量`${MY_VARIABLE}`，`configure_file()`函数将该变量替换为`"Hello, world!"`，

并将结果保存到`config.h`文件中。然后，您可以在`main.cpp`文件中包含`config.h`文件，以便在运行时使用该变量值：

```c++
// main.cpp
#include <iostream>
#include "config.h"

int main() {
  std::cout << MY_VARIABLE << std::endl;
  return 0;
}
```



