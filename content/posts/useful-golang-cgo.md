---
title: "go cgo 简单使用"
# description:
date: 2023-04-20T17:30:48+08:00
draft: false
tags:
- go
- golang
- cgo
categories:
- golang
---


## 介绍

Go是一种快速的编译型语言，它具有高效的内存管理和并发编程的能力。在某些情况下，我们需要使用C或C++的代码库来扩展Go程序的功能。Go通过CGO允许我们在Go代码中调用C函数并在C代码中调用Go函数，这使得Go更加灵活和强大。本文将介绍CGO的简单使用，包括动态和静态库的代码示例及编译运行。

## 简要说明

CGO是Go提供的一个工具，它允许Go代码与C语言代码进行交互。CGO允许我们使用C函数，变量和数据结构，它还可以将Go代码编译成C代码，使得Go和C可以在同一个二进制文件中运行。CGO的一个重要应用是使用现有的C库，以便快速地将其与Go集成在一起。CGO还可以用于将Go代码作为C库的插件。

## 动态库代码示例及编译运行

下面的示例演示了如何使用CGO在Go中调用动态链接库中的C函数。

首先，我们编写一个C语言的源文件，它包含了一个简单的函数。在这个例子中，我们将创建一个名为`sum`的函数，它将两个整数相加并返回结果。

```c
// sum.c
int sum(int a, int b) {
    return a + b;
}
```

接下来，我们需要将这个C文件编译成动态库，以便在Go代码中调用。下面是使用gcc编译器将sum.c编译成动态库的命令：

```shell
gcc -shared -o libsum.so sum.c
```

现在，我们已经有了一个动态链接库，我们可以在Go代码中调用它。下面是一个示例Go代码，它导入C库并调用`sum`函数：

```go
// main.go
package main

// #cgo LDFLAGS: -L. -lsum
// int sum(int a, int b);
import "C"
import "fmt"

func main() {
    a := 1
    b := 2
    c := int(C.sum(C.int(a), C.int(b)))
    fmt.Printf("%d + %d = %d\n", a, b, c)
}
```

让我们来看看这个Go代码的关键部分：

```go
// #cgo LDFLAGS: -L. -lsum
// int sum(int a, int b);
import "C"
```

这个部分告诉CGO我们需要链接一个名为`libsum.so`的库，同时还导入了`sum`函数的声明。

现在我们可以使用以下命令编译和运行这个程序：

```shell
go build && ./main
```

输出应该为：

```
1 + 2 = 3
```

## 静态库代码示例及编译运行

下面的示例演示了如何使用CGO在Go中调用静态链接库中的C函数。

首先，我们编写一个C语言的源文件，它包含了一个简单的函数。在这个例子中，我们将创建一个名为`multiply`的函数，它将两个整数相乘并返回结果。

```c
// multiply.c
int multiply(int a, int b) {
    return a * b;
}
```

接下来，我们需要将这个C文件编译成静态库，以便在Go代码中调用。下面是使用gcc编译器将multiply.c编译成静态库的命令：

```shell
gcc -c multiply.c -o multiply.o
ar rcs libmultiply.a multiply.o
```

现在，我们已经有了一个静态链接库，我们可以在Go代码中调用它。下面是一个示例Go代码，它导入C库并调用`multiply`函数：

```go
// main.go
package main

// #cgo LDFLAGS: -L. -lmultiply
// int multiply(int a, int b);
import "C"
import "fmt"

func main() {
    a := 3
    b := 4
    c := int(C.multiply(C.int(a), C.int(b)))
    fmt.Printf("%d * %d = %d\n", a, b, c)
}
```

让我们来看看这个Go代码的关键部分：

```go
// #cgo LDFLAGS: -L. -lmultiply
// int multiply(int a, int b);
import "C"
```

这个部分告诉CGO我们需要链接一个名为`libmultiply.a`的库，同时还导入了`multiply`函数的声明。

现在我们可以使用以下命令编译和运行这个程序：

```shell
go build && ./main
```

输出应该为：

```
3 * 4 = 12
```

## Go调用C以及C调用Go的示例

在上面的示例中，我们演示了如何在Go中调用C函数，下面我们将演示如何在C代码中调用Go函数。

首先，我们编写一个Go函数，它将返回两个整数的和。

```go
// sum.go
package main

import "C"

//export Sum
func Sum(a, b int) int {
    return a + b
}

func main() {}
```

注意到我们在函数前面加了一个注释：`//export Sum`，这是告诉CGO我们需要将这个函数导出成C函数，以便在C代码中调用。函数名必须是以大写字母开头的。

接下来，我们需要将Go代码编译成动态链接库，以便在C代码中调用。下面是使用go命令将sum.go编译成动态库的命令：

```shell
go build -buildmode=c-shared -o libsum.so sum.go
```

现在，我们已经有了一个动态链接库，我们可以在C代码中调用它。下面是一个示例C代码，它调用了Go代码中的`Sum`函数：

```c
// main.c
#include <stdio.h// declare the function prototype
extern int Sum(int a, int b);

int main() {
    int a = 3;
    int b = 4;
    int c = Sum(a, b);
    printf("%d + %d = %d\n", a, b, c);
    return 0;
}
```

在这个示例中，我们使用`extern`关键字声明了`Sum`函数的原型，然后在`main`函数中调用它。请注意，我们没有包含任何头文件或库文件，因为我们使用CGO直接调用了动态链接库中的函数。

接下来，我们需要将C代码编译成可执行文件，并链接动态链接库。下面是使用gcc编译器将main.c编译成可执行文件的命令：

```shell
gcc -o main main.c -L. -lsum
```

现在，我们可以运行这个可执行文件，并看到输出：

```shell
./main
3 + 4 = 7
```

这个示例演示了如何使用CGO在C代码中调用Go函数，它非常有用，因为有些功能很难在C中实现，但在Go中很容易实现。

## 总结

在本文中，我们介绍了如何使用CGO在Go中调用C函数和在C中调用Go函数。我们使用了静态链接库和动态链接库，演示了在编译和链接时如何使用CGO指令。CGO可以非常方便地在Go和C之间进行交互，但需要注意一些细节。希望本文对你理解CGO有所帮助。
