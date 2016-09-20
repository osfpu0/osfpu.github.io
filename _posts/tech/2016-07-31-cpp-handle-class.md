---
layout: post
title: C++中头文件使用句柄类的好处
category: 技术
tags: C++
keywords: 句柄类
---

在C++中，我们有时候在写一个库的时候，通常声明写在头文件中，定义写在cpp文件中，最后编译之后提供给库的使用者.h文件和库文件（如lib文件），以保护自己的代码，假设我们编写一个Example类如下：

```c++
// example.h
#ifndef ____example__
#define ____example__

class Example {
private:
    int a, b, c;
    void init(int a, int b, int c);
public:
    Example(int a = 0, int b = 0, int c = 0);
    void printInfo();
};

#endif /* defined(____example__) */

----------------------------------------------------------------------------

//example.cpp
#include "example.h"
#include <iostream>

Example::Example(int a, int b, int c) {
    init(a, b, c);
}


void Example::init(int a, int b, int c) {
    this->a = a;
    this->b = b;
    this->c = c;
}

void Example::printInfo() {
    std::cout << "a: " << a << std::endl;
    std::cout << "b: " << b << std::endl;
    std::cout << "c: " << c << std::endl;
}

```
如果恶意程序猿获得Example的头文件和编译后的库，由于看到了example的定义，恶意的按下面方式使用，是可以正常运行的 那么private标识符便失效了，起不到保护作用

```c++
// main.cpp
#define private public
#include "example.h"

int main() {
    Example e(1, 2, 3);
    e.a = 10;
    e.b = 20;
    e.c = 30;
    e.printInfo();
    return 0;
}


```


此时，使用 **句柄类** 的技术便可以避免此类情况。

使用句柄类后的代码如下：

```c++
// handle.h
#ifndef ____handle__
#define ____handle__

class Handle {
private:
    class Example;
    Example *e;
    void init(int a, int b, int c);
public:
    Handle(int a, int b, int c);
    void printInfo();
};

#endif /* defined(____handle__) */

----------------------------------------------------------------------------

//handle.cpp
#include <iostream>
#include "handle.h"

class Handle::Example {
public:
    int a, b, c;
    void init(int a, int b, int c);
    Example(int a = 0, int b = 0, int c = 0);
    void printInfo();
};

Handle::Example::Example(int a, int b, int c) {
    init(a, b, c);
}

void Handle::Example::init(int a, int b, int c) {
    this->a = a;
    this->b = b;
    this->c = c;
}


Handle::Handle(int a, int b, int c) {
    init(a, b, c);
}

void Handle::init(int a, int b, int c) {
    e = new Example(a, b, c);
}

void Handle::printInfo() {
    std::cout << "a: " << e->a << std::endl;
    std::cout << "b: " << e->b << std::endl;
    std::cout << "c: " << e->c << std::endl;
}

```

这样的话，核心定义都会放在cpp文件中，头文件不会暴露太多的信息

并且使用句柄类可以减少重复编译的工作，核心的定义我们可能经常会变动，如果放在头文件中，头文件一变化，包含该头文件的代码文件都要重新编译，如果使用句柄类，改变定义，不会影响句柄类的头文件，只需编译句柄类的cpp文件即可，减少了重复编译。