---
layout: article
title: c&c++_question
tags: c&c++
mathjax: true
key: A-2023-03-26_1


---

c语言的一些问题，参考随想录星球

<!--more-->

***

## c语言写一个尽可能优秀的MAX宏

```c++
#define MAX(a,b) ((a) > (b) ? (a) : (b))
```

该宏使用了条件运算符 `?:`，它是一种三元运算符，如果条件成立则返回第一个表达式的值，否则返回第二个表达式的值。该宏还使用了括号来确保表达式的优先级正确。

```c++
int x = 10;
int y = 20;
int max_value = MAX(x, y);
```

该宏将返回 `y`，因为 `y` 的值比 `x` 大。

注意，该宏只适用于简单类型的比较，如整数和浮点数。对于其他类型，例如字符串，需要使用函数来进行比较。

## c语言中`int a[10][10]` &a+1 和a+1是否相同

可以根据不同维度去理解：&a相当于站在这个数组的上帝视角，&a+1的话就是把整个数组当一个整体，指向了整个数组所占用内存的下一个位置，也就是`&a[10][0]`。a+1的话就相当于把数组中的每一行当做一个整体，给他加一就相当于指向了第二行的首地址，也就是`&a[1][0]`，a[0]+1相当于数组中的一个元素加一，也就是`&a[0][1]`

**不相同**

`&a`表示整个数组`a`的地址，它的类型是`int (*)[10]`（指向包含10个整数的数组的指针）。将`&a`加1会将指针增加整个数组的大小，即`10 * 10 * sizeof(int)`。

而`a`是指向数组`a`的第一个元素的指针，它的类型也是`int (*)[10]`。将`a`加1会将指针增加数组的一个元素的大小，即`10 * sizeof(int)`。

因此，`&a+1`指向整个`a`数组的末尾之后的位置，而`a+1`指向`a`数组的第二行。

![image-20230326103935521](https://vicent-picture-for-typora.oss-cn-beijing.aliyuncs.com/img_for_typora/image-20230326103935521.png)

<img src="https://vicent-picture-for-typora.oss-cn-beijing.aliyuncs.com/img_for_typora/image-20230326103952740.png" alt="image-20230326103952740" style="zoom:150%;" />

## 举例一个使用指向指针的指针俗称的二级指针的场合(void **)

二级指针（指向指针的指针，也称为指针的指针）通常用于需要动态分配内存的情况下，可以通过在函数中传递二级指针来修改函数外部的指针指向的内存区域。一个典型的例子是在C语言中使用动态内存分配函数malloc（）和free（）来动态分配和释放内存。

下面是一个示例，说明如何使用void**（二级指针）来动态分配内存：

```c
#include <stdio.h>
#include <stdlib.h>

void allocate_2d_array(int rows, int cols, void*** array_ptr) {
    int i;
    void** array;
    array = (void**)malloc(rows * sizeof(void*));
    for (i = 0; i < rows; i++) {
        array[i] = (void*)malloc(cols * sizeof(int));
    }
    *array_ptr = array;
}

void free_2d_array(int rows, void*** array_ptr) {
    int i;
    void** array = *array_ptr;
    for (i = 0; i < rows; i++) {
        free(array[i]);
    }
    free(array);
    *array_ptr = NULL;
}

int main() {
    int** array;
    int rows = 3;
    int cols = 4;
    int i, j;
    allocate_2d_array(rows, cols, (void***)&array);
    for (i = 0; i < rows; i++) {
        for (j = 0; j < cols; j++) {
            array[i][j] = i * cols + j;
        }
    }
    for (i = 0; i < rows; i++) {
        for (j = 0; j < cols; j++) {
            printf("%d ", array[i][j]);
        }
        printf("\n");
    }
    free_2d_array(rows, (void***)&array);
    return 0;
}

```

在这个例子中，我们定义了一个`allocate_2d_array`函数来分配一个二维数组，并将指针指向它。这个函数使用二级指针作为它的参数，因为我们需要修改指针的指向。

在`main`函数中，我们定义一个`int**`类型的指针`array`，并将其传递给`allocate_2d_array`函数。在`allocate_2d_array`函数中，我们使用`malloc`函数分配了一个指向指针的指针的数组，然后为每一行分配一个指向`int`类型的指针。最后，我们将指针指向这个新分配的数组。

在我们填充数组并输出它的值之后，我们调用`free_2d_array`函数来释放内存。该函数采用二级指针作为其参数，以便我们能够通过修改指针的指向来释放指向指针的指针的内存。

需要注意的是，在函数`allocate_2d_array`中，我们需要将指针强制转换为`void***`类型，这是因为C语言不允许将`int***`类型的指针

## `int (*p)(void (*)(void *),int**);`解释这个指针的含义

这是一个函数指针，它指向一个函数，该函数接受两个参数，一个是指向函数的指针，该函数指针接受一个指向void的指针作为其参数，并返回void；另一个参数是一个指向指针的指针，该指针指向一个int类型的指针。

更具体地说，这个函数指针的声明是 `int (*p)(void (*)(void *),int**)`，它包含以下部分：

- `(*p)` 表示 `p` 是一个指向函数的指针。
- `(void (*)(void *))` 表示该函数接受一个指向函数的指针，该函数指针接受一个指向void的指针作为其参数，并返回void。这个函数指针的类型是 `void (*)(void *)`。
- `(int**)` 表示该函数接受一个指向指针的指针，该指针指向一个int类型的指针。

因此，`int (*p)(void (*)(void *),int**)` 定义了一个指向这样一种函数的指针。

## 解释这两个宏

```c
#define container_of(ptr, type, member) ({const typeof(((type *)0)->member ) *__mptr = (ptr); (type *)( (char *)__mptr - offsetof(type,member) );})
#define BUILD_BUG(x) (sizeof(struct {int:-!!((x))};)) 
```

```c
#define container_of(ptr, type, member) ({ \    // 开始定义宏，使用 {} 来分隔语句
    const typeof(((type *)0)->member ) *__mptr = (ptr); \    // 声明一个指向 member 成员的指针 __mptr
    (type *)( (char *)__mptr - offsetof(type,member) );})   // 计算结构体指针并返回
```

这是一个 C 语言宏，用于从一个结构体成员的指针中获取结构体的指针。具体来说，它计算出结构体指针相对于成员指针的偏移量，并使用这个偏移量来计算出结构体指针。

具体地：

- `ptr` 是一个指向结构体成员的指针。
- `type` 是结构体的类型。
- `member` 是结构体的一个成员。

这个宏使用了一个 GCC 特有的扩展，即用花括号括起来的一系列语句作为表达式的值。这样可以在宏中使用局部变量，并且允许使用 return 语句返回一个值。

首先，宏使用 typeof 运算符来确定成员指针 `ptr` 的类型，并将其存储在一个名为 `__mptr` 的局部变量中。`typeof` 运算符是一个编译时运算符，它返回一个值的类型。在这里，它返回的是 `member` 成员的类型。

接下来，宏使用 offsetof 宏来计算出结构体指针相对于成员指针的偏移量。`offsetof` 宏是一个标准库宏，它返回一个成员相对于结构体起始地址的偏移量。这里，它返回的是 `member` 成员相对于 `type` 结构体起始地址的偏移量。

最后，宏将指针转换为 char* 类型，并减去偏移量，得到结构体指针。这里使用了指针算术运算，因此必须将指针转换为 char* 类型，以确保减法运算不会产生奇怪的效果。

最终，这个宏的结果是一个指向结构体的指针。

***

```c
#define BUILD_BUG(x) (sizeof(struct { int:-!!((x)); }))
```

这是一个 C 语言宏，用于在编译时产生一个错误。具体来说，它创建一个包含一个不可能为真的条件的匿名结构体，然后使用 `sizeof` 运算符计算该结构体的大小。如果 `x` 是 true，则条件为 !!((x)) = 1，结构体的大小为 4 字节；否则条件为 !!((x)) = 0，结构体的大小为 0 字节。因此，当 `x` 为 true 时，宏展开后会返回一个非零值（4），否则会返回一个零值（0）。

具体地：

- `x` 是一个表达式，用于检查错误。
- 宏定义了一个匿名的结构体，它包含一个有符号整数类型的位域。如果条件为真，则位域的值为 -1，否则为 0。因为位域必须是整数类型，因此使用 int 类型，这意味着该结构体至少占用 4 个字节的空间。
- `!!((x))` 将 x 转换为布尔值，并返回它的相反值。如果 x 是 true，则 !!((x)) 的值为 1；否则为 0。
- `-!!((x))` 将布尔值转换为 -1 或 0。如果 x 是 true，则 -!!((x)) 的值为 -1；否则为 0。
- `sizeof` 运算符返回结构体的大小，即 4 或 0，具体取决于 `x` 是否为 true。由于 `sizeof` 是一个编译时运算符，因此它会在编译时计算结构体的大小，并将其作为整数值返回。这意味着，如果 `x` 为 true，那么在编译时将生成一个大小为 4 字节的结构体，否则将生成一个大小为 0 字节的结构体。

## C语言中如何实现接口

C语言并没有像面向对象语言那样提供直接的接口实现方式。但是，我们可以通过函数指针和结构体来模拟接口的实现。

```c
#include <iostream>
#include <stdio.h>
using namespace std;

// 定义接口类型
typedef struct {
	int (*add)(int a, int b);  // 函数指针
} Interface;

// 实现接口
int add_func(int a, int b) {
	return a + b;
}



int main() {
	// 实例化接口
	Interface interface = {
		 &add_func  // 将函数指针指向实现函数
	};
	// 使用接口
	int result = interface.add(2, 3);
	cout << result << endl;
	return 0;
}
```

在这个示例中，我们首先定义了一个名为`Interface`的结构体类型，其中包含一个名为`add`的函数指针。接着，我们实现了一个名为`add_func`的函数，该函数将两个整数相加并返回它们的和。然后，我们实例化了一个名为`interface`的接口对象，并将函数指针指向实现函数`add_func`。最后，我们使用接口对象调用了`add`函数，将参数2和3传递给它，并将结果存储在名为`result`的变量中。

通过这种方式，我们可以在C语言中实现接口，从而提高代码的可重用性和可维护性。