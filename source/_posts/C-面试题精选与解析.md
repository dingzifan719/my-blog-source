---
title: C++面试题精选与解析
date: 2026-03-05 21:11:47
tags: [C++, 面试, 编程]
---

# C++面试题精选与解析

C++是一门广泛应用于系统开发、游戏编程、嵌入式开发等领域的高级编程语言。在C++面试中，面试官通常会考察候选人对C++核心概念、面向对象编程、内存管理、STL等方面的理解。本文精选了一些常见的C++面试题，并提供详细的解析和代码示例。

## 1. 基本概念

### 1.1 C++与C的区别

**问题**：C++与C语言的主要区别是什么？

**解析**：

1. **面向对象特性**：C++支持面向对象编程（封装、继承、多态），而C是面向过程的语言。
2. **类型检查**：C++有更严格的类型检查。
3. **标准库**：C++提供了更丰富的标准库，包括STL（标准模板库）。
4. **异常处理**：C++支持异常处理（try-catch），而C不支持。
5. **命名空间**：C++支持命名空间（namespace），解决命名冲突问题。
6. **函数重载**：C++支持函数重载，而C不支持。
7. **引用**：C++引入了引用（reference）类型，而C没有。
8. **模板**：C++支持模板（template），实现泛型编程。

### 1.2 头文件与源文件

**问题**：为什么C++代码通常分为头文件（.h）和源文件（.cpp）？

**解析**：

1. **分离声明与实现**：头文件包含类和函数的声明，源文件包含实现，提高代码的可维护性。
2. **避免重复定义**：通过头文件保护（#ifndef/#define/#endif）避免同一文件被多次包含导致的重复定义错误。
3. **提高编译效率**：修改源文件时，只需要重新编译该源文件，而不需要重新编译所有包含该头文件的文件。

**示例**：

```cpp
// Header file: MyClass.h
#ifndef MYCLASS_H
#define MYCLASS_H

class MyClass {
private:
    int m_value;
public:
    MyClass(int value);
    int getValue() const;
    void setValue(int value);
};

#endif

// Source file: MyClass.cpp
#include "MyClass.h"

MyClass::MyClass(int value) : m_value(value) {}

int MyClass::getValue() const {
    return m_value;
}

void MyClass::setValue(int value) {
    m_value = value;
}
```

## 2. 面向对象编程

### 2.1 封装、继承与多态

**问题**：什么是封装、继承和多态？请举例说明。

**解析**：

1. **封装**：将数据和操作数据的方法绑定在一起，对外部隐藏实现细节。
2. **继承**：子类继承父类的属性和方法，实现代码复用和扩展。
3. **多态**：通过基类指针或引用调用派生类的方法，实现动态绑定。

**示例**：

```cpp
#include <iostream>
using namespace std;

// 基类
class Shape {
public:
    // 虚函数，实现多态
    virtual void draw() const {
        cout << "Drawing a shape" << endl;
    }
    
    // 虚析构函数，确保派生类对象能正确析构
    virtual ~Shape() {}
};

// 派生类：Circle
class Circle : public Shape {
public:
    void draw() const override {
        cout << "Drawing a circle" << endl;
    }
};

// 派生类：Rectangle
class Rectangle : public Shape {
public:
    void draw() const override {
        cout << "Drawing a rectangle" << endl;
    }
};

int main() {
    Shape* shape1 = new Circle();
    Shape* shape2 = new Rectangle();
    
    // 多态：调用派生类的draw方法
    shape1->draw();  // 输出：Drawing a circle
    shape2->draw();  // 输出：Drawing a rectangle
    
    delete shape1;
    delete shape2;
    
    return 0;
}
```

### 2.2 虚函数与纯虚函数

**问题**：什么是虚函数和纯虚函数？它们的区别是什么？

**解析**：

1. **虚函数**：在基类中使用`virtual`关键字声明的函数，允许派生类重写，并通过基类指针或引用调用时实现动态绑定。
2. **纯虚函数**：在基类中声明但没有实现的虚函数，使用`= 0`语法表示，要求派生类必须实现该函数。

**区别**：
- 虚函数有默认实现，纯虚函数没有。
- 包含纯虚函数的类是抽象类，不能实例化；包含虚函数的类可以实例化。
- 抽象类的派生类必须实现所有纯虚函数才能实例化。

**示例**：

```cpp
#include <iostream>
using namespace std;

// 抽象类：包含纯虚函数
class AbstractShape {
public:
    // 纯虚函数
    virtual void draw() const = 0;
    
    // 虚析构函数
    virtual ~AbstractShape() {}
};

// 具体类：实现纯虚函数
class Circle : public AbstractShape {
public:
    void draw() const override {
        cout << "Drawing a circle" << endl;
    }
};

int main() {
    // AbstractShape shape;  // 错误：不能实例化抽象类
    AbstractShape* shape = new Circle();
    shape->draw();  // 输出：Drawing a circle
    
    delete shape;
    return 0;
}
```

## 3. 内存管理

### 3.1 堆与栈

**问题**：C++中的堆内存和栈内存有什么区别？

**解析**：

| 特性 | 栈内存 | 堆内存 |
|------|--------|--------|
| 分配方式 | 自动分配和释放 | 手动分配和释放 |
| 空间大小 | 较小（通常几MB） | 较大（通常几GB） |
| 分配速度 | 快 | 慢 |
| 内存碎片 | 无 | 有 |
| 访问方式 | 直接访问 | 通过指针访问 |
| 生命周期 | 作用域结束时自动释放 | 手动释放或程序结束时释放 |

**示例**：

```cpp
#include <iostream>
using namespace std;

void func() {
    int stackVar = 10;  // 栈内存
    int* heapVar = new int(20);  // 堆内存
    
    cout << "Stack variable: " << stackVar << endl;
    cout << "Heap variable: " << *heapVar << endl;
    
    delete heapVar;  // 手动释放堆内存
}

int main() {
    func();
    return 0;
}
```

### 3.2 智能指针

**问题**：什么是智能指针？C++11提供了哪些智能指针？

**解析**：

智能指针是C++标准库提供的模板类，用于自动管理动态分配的内存，避免内存泄漏。C++11提供了三种智能指针：

1. **unique_ptr**：独占式智能指针，同一时间只能有一个unique_ptr指向同一个对象。
2. **shared_ptr**：共享式智能指针，允许多个shared_ptr指向同一个对象，使用引用计数管理对象生命周期。
3. **weak_ptr**：弱引用智能指针，不增加引用计数，用于解决shared_ptr的循环引用问题。

**示例**：

```cpp
#include <iostream>
#include <memory>
using namespace std;

class MyClass {
public:
    MyClass(int value) : m_value(value) {
        cout << "Constructor called: " << m_value << endl;
    }
    
    ~MyClass() {
        cout << "Destructor called: " << m_value << endl;
    }
    
    int getValue() const { return m_value; }
    
private:
    int m_value;
};

int main() {
    // unique_ptr示例
    cout << "=== unique_ptr example ===" << endl;
    unique_ptr<MyClass> ptr1(new MyClass(1));
    // unique_ptr<MyClass> ptr2 = ptr1;  // 错误：不能复制unique_ptr
    unique_ptr<MyClass> ptr2 = move(ptr1);  // 可以移动unique_ptr
    cout << "Value: " << ptr2->getValue() << endl;
    
    // shared_ptr示例
    cout << "\n=== shared_ptr example ===" << endl;
    shared_ptr<MyClass> ptr3(new MyClass(2));
    shared_ptr<MyClass> ptr4 = ptr3;  // 可以复制shared_ptr
    cout << "Value: " << ptr3->getValue() << endl;
    cout << "Reference count: " << ptr3.use_count() << endl;
    
    // weak_ptr示例
    cout << "\n=== weak_ptr example ===" << endl;
    weak_ptr<MyClass> weakPtr = ptr3;
    cout << "Reference count: " << ptr3.use_count() << endl;
    if (auto shared = weakPtr.lock()) {
        cout << "Value: " << shared->getValue() << endl;
    }
    
    return 0;
}
```

### 3.3 内存泄漏

**问题**：什么是内存泄漏？如何避免内存泄漏？

**解析**：

内存泄漏是指程序中动态分配的内存没有被正确释放，导致内存资源浪费。

避免内存泄漏的方法：
1. 尽量使用栈内存，减少堆内存的使用。
2. 使用智能指针管理堆内存。
3. 确保每个`new`都有对应的`delete`，每个`new[]`都有对应的`delete[]`。
4. 使用RAII（资源获取即初始化）原则管理资源。
5. 使用内存泄漏检测工具，如Valgrind、AddressSanitizer等。

## 4. 模板与STL

### 4.1 模板

**问题**：什么是模板？模板有哪些类型？

**解析**：

模板是C++支持泛型编程的核心机制，允许定义通用的函数和类，支持多种数据类型。

模板分为两种类型：
1. **函数模板**：定义通用函数，支持多种参数类型。
2. **类模板**：定义通用类，支持多种数据类型。

**示例**：

```cpp
#include <iostream>
using namespace std;

// 函数模板
 template <typename T>
T maxValue(T a, T b) {
    return (a > b) ? a : b;
}

// 类模板
 template <typename T>
class Stack {
private:
    T* m_array;
    int m_top;
    int m_capacity;
    
public:
    Stack(int capacity = 10) : m_capacity(capacity), m_top(-1) {
        m_array = new T[capacity];
    }
    
    ~Stack() {
        delete[] m_array;
    }
    
    void push(const T& value) {
        if (m_top >= m_capacity - 1) {
            // 扩容逻辑（简化示例，未实现）
            return;
        }
        m_array[++m_top] = value;
    }
    
    T pop() {
        if (m_top < 0) {
            // 错误处理（简化示例，未实现）
            return T();
        }
        return m_array[m_top--];
    }
    
    bool isEmpty() const {
        return m_top < 0;
    }
};

int main() {
    // 函数模板示例
    int a = 10, b = 20;
    cout << "Max of " << a << " and " << b << " is " << maxValue(a, b) << endl;
    
    double c = 3.14, d = 2.71;
    cout << "Max of " << c << " and " << d << " is " << maxValue(c, d) << endl;
    
    // 类模板示例
    Stack<int> intStack;
    intStack.push(1);
    intStack.push(2);
    intStack.push(3);
    
    while (!intStack.isEmpty()) {
        cout << "Popped: " << intStack.pop() << endl;
    }
    
    return 0;
}
```

### 4.2 STL容器

**问题**：STL提供了哪些常用容器？请举例说明。

**解析**：

STL（标准模板库）提供了多种容器，用于存储和管理数据。常用的容器包括：

1. **序列容器**：
   - `vector`：动态数组
   - `list`：双向链表
   - `forward_list`：单向链表
   - `deque`：双端队列
   - `array`：固定大小数组

2. **关联容器**：
   - `set`：有序集合
   - `multiset`：有序多重集合
   - `map`：有序键值对
   - `multimap`：有序多重键值对

3. **无序容器**：
   - `unordered_set`：哈希集合
   - `unordered_multiset`：哈希多重集合
   - `unordered_map`：哈希键值对
   - `unordered_multimap`：哈希多重键值对

4. **适配器容器**：
   - `stack`：栈
   - `queue`：队列
   - `priority_queue`：优先队列

**示例**：

```cpp
#include <iostream>
#include <vector>
#include <list>
#include <map>
#include <set>
#include <unordered_map>
#include <algorithm>
using namespace std;

int main() {
    // vector示例
    cout << "=== vector example ===" << endl;
    vector<int> vec = {1, 2, 3, 4, 5};
    vec.push_back(6);
    for (int num : vec) {
        cout << num << " ";
    }
    cout << endl;
    
    // list示例
    cout << "\n=== list example ===" << endl;
    list<int> lst = {10, 20, 30};
    lst.push_front(5);
    lst.push_back(40);
    for (int num : lst) {
        cout << num << " ";
    }
    cout << endl;
    
    // map示例
    cout << "\n=== map example ===" << endl;
    map<string, int> scores;
    scores["Alice"] = 90;
    scores["Bob"] = 85;
    scores["Charlie"] = 95;
    
    for (const auto& pair : scores) {
        cout << pair.first << ": " << pair.second << endl;
    }
    
    // set示例
    cout << "\n=== set example ===" << endl;
    set<int> nums = {5, 2, 8, 1, 9};
    nums.insert(3);
    for (int num : nums) {
        cout << num << " ";
    }
    cout << endl;
    
    // unordered_map示例
    cout << "\n=== unordered_map example ===" << endl;
    unordered_map<string, string> capitals;
    capitals["China"] = "Beijing";
    capitals["USA"] = "Washington D.C.";
    capitals["Japan"] = "Tokyo";
    
    for (const auto& pair : capitals) {
        cout << pair.first << ": " << pair.second << endl;
    }
    
    return 0;
}
```

## 5. 高级特性

### 5.1 右值引用与移动语义

**问题**：什么是右值引用和移动语义？它们解决了什么问题？

**解析**：

1. **右值引用**：使用`&&`表示的引用类型，用于绑定到右值（临时对象）。
2. **移动语义**：允许资源（如内存）从一个对象转移到另一个对象，避免不必要的拷贝。

它们解决的问题：
- 减少不必要的对象拷贝，提高性能。
- 解决临时对象的资源浪费问题。

**示例**：

```cpp
#include <iostream>
#include <vector>
#include <string>
using namespace std;

class MyString {
private:
    char* m_data;
    size_t m_length;
    
public:
    // 默认构造函数
    MyString() : m_data(nullptr), m_length(0) {
        cout << "Default constructor" << endl;
    }
    
    // 构造函数
    MyString(const char* str) {
        cout << "Constructor" << endl;
        m_length = strlen(str);
        m_data = new char[m_length + 1];
        strcpy(m_data, str);
    }
    
    // 拷贝构造函数
    MyString(const MyString& other) {
        cout << "Copy constructor" << endl;
        m_length = other.m_length;
        m_data = new char[m_length + 1];
        strcpy(m_data, other.m_data);
    }
    
    // 移动构造函数
    MyString(MyString&& other) noexcept : m_data(other.m_data), m_length(other.m_length) {
        cout << "Move constructor" << endl;
        other.m_data = nullptr;
        other.m_length = 0;
    }
    
    // 析构函数
    ~MyString() {
        cout << "Destructor" << endl;
        delete[] m_data;
    }
    
    // 拷贝赋值运算符
    MyString& operator=(const MyString& other) {
        cout << "Copy assignment operator" << endl;
        if (this != &other) {
            delete[] m_data;
            m_length = other.m_length;
            m_data = new char[m_length + 1];
            strcpy(m_data, other.m_data);
        }
        return *this;
    }
    
    // 移动赋值运算符
    MyString& operator=(MyString&& other) noexcept {
        cout << "Move assignment operator" << endl;
        if (this != &other) {
            delete[] m_data;
            m_data = other.m_data;
            m_length = other.m_length;
            other.m_data = nullptr;
            other.m_length = 0;
        }
        return *this;
    }
    
    const char* c_str() const {
        return m_data ? m_data : "";
    }
};

int main() {
    cout << "=== Creating strings ===" << endl;
    MyString s1("Hello");
    MyString s2("World");
    
    cout << "\n=== Copy assignment ===" << endl;
    MyString s3 = s1;  // 拷贝构造
    
    cout << "\n=== Move assignment ===" << endl;
    MyString s4 = move(s2);  // 移动构造
    
    cout << "\n=== String values ===" << endl;
    cout << "s1: " << s1.c_str() << endl;
    cout << "s2: " << s2.c_str() << endl;
    cout << "s3: " << s3.c_str() << endl;
    cout << "s4: " << s4.c_str() << endl;
    
    return 0;
}
```

### 5.2 Lambda表达式

**问题**：什么是Lambda表达式？如何使用Lambda表达式？

**解析**：

Lambda表达式是C++11引入的特性，允许在代码中定义匿名函数，简化代码编写。

Lambda表达式的语法：cpp
[capture](parameters) -> return_type { body }

- `capture`：捕获列表，用于访问外部变量。
- `parameters`：参数列表。
- `return_type`：返回类型（可选）。
- `body`：函数体。

**示例**：

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
#include <functional>
using namespace std;

int main() {
    // 基本Lambda表达式
    cout << "=== Basic lambda ===" << endl;
    auto add = [](int a, int b) { return a + b; };
    cout << "5 + 3 = " << add(5, 3) << endl;
    
    // 带捕获列表的Lambda表达式
    cout << "\n=== Lambda with capture ===" << endl;
    int x = 10;
    auto multiply = [x](int y) { return x * y; };
    cout << "10 * 5 = " << multiply(5) << endl;
    
    // 引用捕获
    auto increment = [&x]() { x++; };
    increment();
    cout << "After increment: x = " << x << endl;
    
    // 在STL中使用Lambda
    cout << "\n=== Lambda in STL ===" << endl;
    vector<int> nums = {5, 2, 8, 1, 9, 3};
    
    // 排序
    sort(nums.begin(), nums.end(), [](int a, int b) { return a > b; });
    cout << "Sorted in descending order: ";
    for (int num : nums) {
        cout << num << " ";
    }
    cout << endl;
    
    // 查找
    auto it = find_if(nums.begin(), nums.end(), [](int num) { return num % 2 == 0; });
    if (it != nums.end()) {
        cout << "First even number: " << *it << endl;
    }
    
    // 计数
    int count = 0;
    for_each(nums.begin(), nums.end(), [&count](int num) { 
        if (num > 5) count++; 
    });
    cout << "Numbers greater than 5: " << count << endl;
    
    return 0;
}
```

## 6. 常见算法与数据结构

### 6.1 排序算法

**问题**：C++中常用的排序算法有哪些？它们的时间复杂度分别是多少？

**解析**：

C++标准库提供了多种排序算法，常用的包括：

1. **sort**：基于快速排序，平均时间复杂度O(n log n)，最坏情况O(n²)。
2. **stable_sort**：稳定排序，基于归并排序，时间复杂度O(n log n)。
3. **partial_sort**：部分排序，时间复杂度O(n log k)，其中k是要排序的元素数量。
4. **nth_element**：找到第n个元素，时间复杂度O(n)。

**示例**：

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

int main() {
    vector<int> nums = {5, 2, 8, 1, 9, 3, 7, 4, 6};
    
    // 排序
    cout << "=== sort ===" << endl;
    sort(nums.begin(), nums.end());
    for (int num : nums) {
        cout << num << " ";
    }
    cout << endl;
    
    // 降序排序
    cout << "\n=== sort (descending) ===" << endl;
    sort(nums.begin(), nums.end(), greater<int>());
    for (int num : nums) {
        cout << num << " ";
    }
    cout << endl;
    
    // 部分排序（前3个最小元素）
    cout << "\n=== partial_sort ===" << endl;
    partial_sort(nums.begin(), nums.begin() + 3, nums.end());
    for (int num : nums) {
        cout << num << " ";
    }
    cout << endl;
    
    // 找到第5个元素（0-based）
    cout << "\n=== nth_element ===" << endl;
    nth_element(nums.begin(), nums.begin() + 4, nums.end());
    cout << "5th element: " << nums[4] << endl;
    
    return 0;
}
```

### 6.2 链表与树

**问题**：如何实现一个单链表？如何反转一个单链表？

**解析**：

单链表是一种常见的数据结构，每个节点包含数据和指向下一个节点的指针。

**示例**：

```cpp
#include <iostream>
using namespace std;

// 链表节点定义
struct ListNode {
    int val;
    ListNode* next;
    ListNode(int x) : val(x), next(nullptr) {}
};

// 反转单链表
ListNode* reverseList(ListNode* head) {
    ListNode* prev = nullptr;
    ListNode* curr = head;
    
    while (curr != nullptr) {
        ListNode* nextTemp = curr->next;  // 保存下一个节点
        curr->next = prev;  // 反转指针
        prev = curr;  // 移动prev指针
        curr = nextTemp;  // 移动curr指针
    }
    
    return prev;  // 新的头节点
}

// 打印链表
void printList(ListNode* head) {
    ListNode* curr = head;
    while (curr != nullptr) {
        cout << curr->val << " -> ";
        curr = curr->next;
    }
    cout << "nullptr" << endl;
}

// 释放链表内存
void deleteList(ListNode* head) {
    ListNode* curr = head;
    while (curr != nullptr) {
        ListNode* temp = curr;
        curr = curr->next;
        delete temp;
    }
}

int main() {
    // 创建链表：1 -> 2 -> 3 -> 4 -> 5
    ListNode* head = new ListNode(1);
    head->next = new ListNode(2);
    head->next->next = new ListNode(3);
    head->next->next->next = new ListNode(4);
    head->next->next->next->next = new ListNode(5);
    
    cout << "Original list: " << endl;
    printList(head);
    
    // 反转链表
    ListNode* reversedHead = reverseList(head);
    
    cout << "Reversed list: " << endl;
    printList(reversedHead);
    
    // 释放内存
    deleteList(reversedHead);
    
    return 0;
}
```

## 7. 代码优化与性能

### 7.1 性能优化技巧

**问题**：C++代码有哪些常见的性能优化技巧？

**解析**：

1. **减少内存分配和释放**：使用对象池、避免频繁的new/delete操作。
2. **减少拷贝操作**：使用引用和指针、移动语义。
3. **使用更高效的算法和数据结构**：根据实际需求选择合适的算法和数据结构。
4. **循环优化**：减少循环内的计算、使用循环展开、避免分支预测失败。
5. **内存对齐**：合理安排结构体成员顺序，提高内存访问效率。
6. **避免虚函数调用**：在性能敏感的代码中，避免频繁的虚函数调用。
7. **使用内联函数**：对于短小的函数，使用inline关键字提高性能。
8. **使用const和constexpr**：帮助编译器进行优化。
9. **避免内存泄漏**：及时释放内存，避免资源浪费。
10. **使用编译器优化选项**：如-O2、-O3等。

### 7.2 内存对齐

**问题**：什么是内存对齐？为什么要进行内存对齐？

**解析**：

内存对齐是指数据在内存中的地址必须是某个值的倍数，这个值称为对齐模数。

内存对齐的原因：
1. **提高内存访问效率**：计算机以块为单位访问内存，对齐的数据可以一次读取完成，提高效率。
2. **硬件要求**：某些硬件平台要求数据必须对齐，否则会产生硬件异常。
3. **兼容性**：不同的硬件平台有不同的对齐要求，对齐可以提高代码的兼容性。

**示例**：

```cpp
#include <iostream>
using namespace std;

// 未优化的结构体
struct UnoptimizedStruct {
    char c;
    int i;
    double d;
    short s;
};

// 优化的结构体（按大小排序）
struct OptimizedStruct {
    double d;
    int i;
    short s;
    char c;
};

int main() {
    cout << "Size of char: " << sizeof(char) << " byte" << endl;
    cout << "Size of short: " << sizeof(short) << " bytes" << endl;
    cout << "Size of int: " << sizeof(int) << " bytes" << endl;
    cout << "Size of double: " << sizeof(double) << " bytes" << endl;
    cout << endl;
    
    cout << "Size of UnoptimizedStruct: " << sizeof(UnoptimizedStruct) << " bytes" << endl;
    cout << "Size of OptimizedStruct: " << sizeof(OptimizedStruct) << " bytes" << endl;
    
    return 0;
}
```
## 总结

本文精选了C++面试中常见的问题，涵盖了基本概念、面向对象编程、内存管理、模板与STL、高级特性、常见算法与数据结构、代码优化与性能等方面。每个问题都提供了详细的解析和代码示例，帮助读者理解和掌握C++的核心知识。

C++是一门复杂而强大的语言，需要不断学习和实践才能掌握。希望本文能够帮助读者在C++面试中取得好成绩，同时也能够提高读者的C++编程水平。

Good luck with your interviews! 🚀