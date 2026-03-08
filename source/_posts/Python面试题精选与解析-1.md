---
title: Python面试题精选与解析
date: 2026-03-05 21:41:37
tags: [Python, 面试, 编程]
---

# Python面试题精选与解析

Python是一门广泛应用于Web开发、数据分析、人工智能、自动化运维等领域的高级编程语言。在Python面试中，面试官通常会考察候选人对Python核心概念、数据结构、面向对象编程、并发编程等方面的理解。本文精选了一些常见的Python面试题，并提供详细的解析和代码示例。

## 1. Python基础

### 1.1 Python的特点

**问题**：Python有哪些主要特点？

**解析**：

1. **简单易学**：语法简洁清晰，接近自然语言，易于学习和使用。
2. **解释型语言**：无需编译，直接运行源代码，便于调试和开发。
3. **动态类型**：变量类型在运行时确定，无需显式声明。
4. **面向对象**：支持类、继承、多态等面向对象特性。
5. **丰富的标准库**：提供大量实用模块，涵盖文件处理、网络编程、数据库访问等。
6. **可扩展性**：支持C/C++扩展，可与其他语言集成。
7. **跨平台**：可在Windows、macOS、Linux等多种操作系统上运行。

### 1.2 深拷贝与浅拷贝

**问题**：什么是深拷贝和浅拷贝？它们的区别是什么？

**解析**：

- **浅拷贝**：创建一个新的对象，但只复制原始对象的引用，而不复制引用的对象。因此，原始对象和拷贝对象共享相同的引用对象。
- **深拷贝**：创建一个新的对象，并递归地复制原始对象及其引用的所有对象。因此，原始对象和拷贝对象完全独立。

**示例**：

```python
import copy

# 原始数据
original = {
    'name': 'Alice',
    'age': 25,
    'hobbies': ['reading', 'swimming']
}

# 浅拷贝
shallow_copy = copy.copy(original)

# 深拷贝
deep_copy = copy.deepcopy(original)

# 修改原始数据中的可变对象
original['hobbies'].append('coding')

print("Original:", original)
print("Shallow copy:", shallow_copy)
print("Deep copy:", deep_copy)
```

**输出**：

```
Original: {'name': 'Alice', 'age': 25, 'hobbies': ['reading', 'swimming', 'coding']}
Shallow copy: {'name': 'Alice', 'age': 25, 'hobbies': ['reading', 'swimming', 'coding']}
Deep copy: {'name': 'Alice', 'age': 25, 'hobbies': ['reading', 'swimming']}
```

### 1.3 装饰器

**问题**：什么是装饰器？请举例说明。

**解析**：

装饰器是Python中的一种高级特性，用于在不修改函数源代码的情况下，为函数添加额外的功能。装饰器本质上是一个高阶函数，它接收一个函数作为参数，并返回一个新的函数。

**示例**：

```python
import functools
import time

# 定义一个简单的装饰器
def my_decorator(func):
    @functools.wraps(func)
    def wrapper(*args, **kwargs):
        print(f"Before calling {func.__name__}")
        result = func(*args, **kwargs)
        print(f"After calling {func.__name__}")
        return result
    return wrapper

# 使用装饰器
@my_decorator
def say_hello(name):
    print(f"Hello, {name}!")

# 调用函数
say_hello("Alice")

# 带参数的装饰器
def repeat(num):
    def decorator(func):
        @functools.wraps(func)
        def wrapper(*args, **kwargs):
            for _ in range(num):
                result = func(*args, **kwargs)
            return result
        return wrapper
    return decorator

@repeat(3)
def greet():
    print("Greetings!")

greet()

# 性能计时装饰器
def timer(func):
    @functools.wraps(func)
    def wrapper(*args, **kwargs):
        start_time = time.time()
        result = func(*args, **kwargs)
        end_time = time.time()
        print(f"{func.__name__} took {end_time - start_time:.4f} seconds")
        return result
    return wrapper

@timer
def slow_function():
    time.sleep(1)

slow_function()
```

## 2. 数据结构与算法

### 2.1 列表、元组、字典、集合的区别

**问题**：Python中的列表（list）、元组（tuple）、字典（dict）、集合（set）有什么区别？

**解析**：

| 特性 | 列表（list） | 元组（tuple） | 字典（dict） | 集合（set） |
|------|-------------|--------------|-------------|------------|
| 可变性 | 可变 | 不可变 | 可变 | 可变 |
| 有序性 | 有序 | 有序 | 无序（Python 3.7+ 有序） | 无序 |
| 重复元素 | 允许 | 允许 | 键不允许，值允许 | 不允许 |
| 索引访问 | 支持 | 支持 | 通过键访问 | 不支持 |
| 语法 | `[1, 2, 3]` | `(1, 2, 3)` | `{'a': 1}` | `{1, 2, 3}` |

**示例**：

```python
# 列表示例
my_list = [1, 2, 3, 4, 5]
my_list.append(6)  # 添加元素
my_list[0] = 100   # 修改元素
print(f"List: {my_list}")

# 元组示例
my_tuple = (1, 2, 3)
# my_tuple[0] = 100  # 错误：元组不可变
print(f"Tuple: {my_tuple}")

# 字典示例
my_dict = {'name': 'Alice', 'age': 25}
my_dict['city'] = 'Beijing'  # 添加键值对
print(f"Dict: {my_dict}")

# 集合示例
my_set = {1, 2, 3, 4, 5}
my_set.add(6)  # 添加元素
my_set.add(1)  # 重复元素不会被添加
print(f"Set: {my_set}")

# 集合运算
set1 = {1, 2, 3, 4}
set2 = {3, 4, 5, 6}
print(f"Union: {set1 | set2}")
print(f"Intersection: {set1 & set2}")
print(f"Difference: {set1 - set2}")
```

### 2.2 列表推导式

**问题**：什么是列表推导式？如何使用列表推导式？

**解析**：

列表推导式是Python中的一种简洁语法，用于从一个或多个可迭代对象创建新的列表。它比传统的for循环更简洁、更易读。

基本语法：python
[expression for item in iterable]
[expression for item in iterable if condition]

**示例**：

```python
# 基本列表推导式
numbers = [1, 2, 3, 4, 5]
squares = [x**2 for x in numbers]
print(f"Squares: {squares}")

# 带条件的列表推导式
even_squares = [x**2 for x in numbers if x % 2 == 0]
print(f"Even squares: {even_squares}")

# 处理字符串
words = ["hello", "world", "python"]
upper_words = [word.upper() for word in words]
print(f"Upper words: {upper_words}")

# 嵌套列表推导式
matrix = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
flattened = [item for row in matrix for item in row]
print(f"Flattened: {flattened}")

# 字典推导式
keys = ['a', 'b', 'c']
values = [1, 2, 3]
dict_comp = {k: v for k, v in zip(keys, values)}
print(f"Dict: {dict_comp}")

# 集合推导式
set_comp = {x**2 for x in range(10)}
print(f"Set: {set_comp}")
```

### 2.3 生成器

**问题**：什么是生成器？生成器和迭代器有什么区别？

**解析**：

生成器是一种特殊的迭代器，它允许你逐个生成值，而不是一次性生成所有值。生成器使用`yield`关键字来返回值，并在每次调用时保存当前状态，下次调用时从上次离开的地方继续执行。

生成器与迭代器的区别：
- **迭代器**：实现了`__iter__()`和`__next__()`方法的对象。
- **生成器**：一种特殊的迭代器，使用`yield`关键字定义，更加简洁高效。

**示例**：

```python
# 使用函数定义生成器
def fibonacci(n):
    """生成斐波那契数列的前n个数"""
    a, b = 0, 1
    for _ in range(n):
        yield a
        a, b = b, a + b

# 使用生成器
print("Fibonacci sequence:")
for num in fibonacci(10):
    print(num, end=" ")
print()

# 使用生成器表达式
squares_gen = (x**2 for x in range(10))
print(f"\nSquares (generator): {list(squares_gen)}")

# 生成器与列表的区别
def infinite_counter(start=0):
    """无限计数器生成器"""
    while True:
        yield start
        start += 1

# 使用无限生成器（只会生成前5个值）
counter = infinite_counter(100)
print("\nInfinite counter (first 5 values):")
for _ in range(5):
    print(next(counter), end=" ")
print()

# 生成器的状态保存
def countdown(n):
    """倒计时生成器"""
    print(f"Starting countdown from {n}")
    while n > 0:
        yield n
        n -= 1
    print("Countdown finished!")

print("\nCountdown:")
cd = countdown(3)
for value in cd:
    print(f"Count: {value}")

# 使用send方法与生成器通信
def accumulator():
    """累加器生成器，可以接收外部发送的值"""
    total = 0
    while True:
        value = yield total
        if value is None:
            break
        total += value

print("\nAccumulator:")
acc = accumulator()
next(acc)  # 启动生成器
print(f"Add 10: {acc.send(10)}")
print(f"Add 20: {acc.send(20)}")
print(f"Add 30: {acc.send(30)}")
acc.close()
```
## 3. 面向对象编程

### 3.1 类与对象

**问题**：Python中的类与对象是什么关系？如何定义一个类？

**解析**：

- **类（Class）**：是一种抽象的数据类型，定义了对象的属性和方法。类是创建对象的模板。
- **对象（Object）**：是类的实例，具有类定义的属性和方法。

定义类的基本语法：

```python
class ClassName:
    # 类属性
    class_attribute = value
    
    # 构造方法
    def __init__(self, param1, param2):
        self.param1 = param1
        self.param2 = param2
    
    # 实例方法
    def method(self):
        pass
    
    # 类方法
    @classmethod
    def class_method(cls):
        pass
    
    # 静态方法
    @staticmethod
    def static_method():
        pass
```
**示例**：

```python
class Person:
    # 类属性
    species = "Homo sapiens"
    count = 0
    
    def __init__(self, name, age):
        """构造方法"""
        self.name = name
        self._age = age  # 保护属性
        Person.count += 1
    
    def introduce(self):
        """实例方法"""
        return f"Hi, I'm {self.name}, {self._age} years old."
    
    def have_birthday(self):
        """实例方法"""
        self._age += 1
        return f"Happy birthday, {self.name}! Now you're {self._age}."
    
    @classmethod
    def create_anonymous(cls):
        """类方法"""
        return cls("Anonymous", 0)
    
    @classmethod
    def get_count(cls):
        """类方法"""
        return cls.count
    
    @staticmethod
    def is_adult(age):
        """静态方法"""
        return age >= 18
    
    @property
    def age(self):
        """属性装饰器"""
        return self._age
    
    @age.setter
    def age(self, value):
        if value < 0:
            raise ValueError("Age cannot be negative")
        self._age = value
    
    def __str__(self):
        """字符串表示"""
        return f"Person(name='{self.name}', age={self._age})"
    
    def __repr__(self):
        """官方字符串表示"""
        return f"Person('{self.name}', {self._age})"
    
    def __eq__(self, other):
        """相等比较"""
        if not isinstance(other, Person):
            return False
        return self.name == other.name and self._age == other._age
    
    def __lt__(self, other):
        """小于比较"""
        if not isinstance(other, Person):
            return NotImplemented
        return self._age < other._age

# 使用示例
print("=== Creating Person objects ===")
person1 = Person("Alice", 25)
person2 = Person("Bob", 30)

print(f"Person 1: {person1}")
print(f"Person 2: {person2}")

print(f"\nIntroduction: {person1.introduce()}")
print(f"Birthday message: {person1.have_birthday()}")

print(f"\nTotal persons created: {Person.get_count()}")

# 类方法
anonymous = Person.create_anonymous()
print(f"Anonymous person: {anonymous}")

# 静态方法
print(f"\nIs 25 an adult? {Person.is_adult(25)}")
print(f"Is 16 an adult? {Person.is_adult(16)}")

# 属性装饰器
print(f"\nAlice's age (property): {person1.age}")
person1.age = 26
print(f"Alice's new age: {person1.age}")

# 比较
person3 = Person("Alice", 26)
print(f"\nPerson 1 == Person 3: {person1 == person3}")
print(f"Person 1 < Person 2: {person1 < person2}")

# 字符串表示
print(f"\nstr(): {str(person1)}")
print(f"repr(): {repr(person1)}")
```

### 3.2 继承与多态

**问题**：Python中如何实现继承和多态？

**解析**：

- **继承**：子类可以继承父类的属性和方法，并可以添加新的属性和方法或重写父类的方法。
- **多态**：不同的子类对象调用相同的方法，表现出不同的行为。

**示例**：

```python
from abc import ABC, abstractmethod
import math

# 抽象基类
class Shape(ABC):
    """形状抽象基类"""
    
    @abstractmethod
    def area(self):
        """计算面积"""
        pass
    
    @abstractmethod
    def perimeter(self):
        """计算周长"""
        pass
    
    def describe(self):
        """描述形状"""
        return f"This is a {self.__class__.__name__}"

# 具体形状类
class Circle(Shape):
    """圆形"""
    
    def __init__(self, radius):
        self.radius = radius
    
    def area(self):
        return math.pi * self.radius ** 2
    
    def perimeter(self):
        return 2 * math.pi * self.radius
    
    def __str__(self):
        return f"Circle(radius={self.radius})"

class Rectangle(Shape):
    """矩形"""
    
    def __init__(self, width, height):
        self.width = width
        self.height = height
    
    def area(self):
        return self.width * self.height
    
    def perimeter(self):
        return 2 * (self.width + self.height)
    
    def __str__(self):
        return f"Rectangle(width={self.width}, height={self.height})"

class Triangle(Shape):
    """三角形"""
    
    def __init__(self, a, b, c):
        self.a = a
        self.b = b
        self.c = c
    
    def area(self):
        # 使用海伦公式
        s = (self.a + self.b + self.c) / 2
        return math.sqrt(s * (s - self.a) * (s - self.b) * (s - self.c))
    
    def perimeter(self):
        return self.a + self.b + self.c
    
    def __str__(self):
        return f"Triangle(a={self.a}, b={self.b}, c={self.c})"

# 使用多态
class ShapeCalculator:
    """形状计算器"""
    
    @staticmethod
    def print_shape_info(shape):
        """打印形状信息（多态）"""
        print(f"\nShape: {shape}")
        print(f"Description: {shape.describe()}")
        print(f"Area: {shape.area():.2f}")
        print(f"Perimeter: {shape.perimeter():.2f}")
    
    @staticmethod
    def total_area(shapes):
        """计算多个形状的总面积"""
        return sum(shape.area() for shape in shapes)

# 使用示例
print("=== Python继承与多态示例 ===\n")

# 创建各种形状
circle = Circle(5)
rectangle = Rectangle(4, 6)
triangle = Triangle(3, 4, 5)

# 使用多态打印形状信息
shapes = [circle, rectangle, triangle]
for shape in shapes:
    ShapeCalculator.print_shape_info(shape)

# 计算总面积
total = ShapeCalculator.total_area(shapes)
print(f"\nTotal area of all shapes: {total:.2f}")

# 类型检查
print("\n=== Type checking ===")
for shape in shapes:
    print(f"{shape} is instance of Shape: {isinstance(shape, Shape)}")
    print(f"{shape} is instance of Circle: {isinstance(shape, Circle)}")
```

## 4. 并发编程

### 4.1 多线程与多进程

**问题**：Python中的多线程和多进程有什么区别？如何选择使用？

**解析**：

| 特性 | 多线程（threading） | 多进程（multiprocessing） |
|------|-------------------|-------------------------|
| **执行方式** | 同一进程内的多个线程共享内存空间 | 独立的进程，拥有独立的内存空间 |
| **GIL影响** | 受GIL限制，不能真正实现并行 | 不受GIL限制，可以实现真正并行 |
| **适用场景** | I/O密集型任务（网络请求、文件操作） | CPU密集型任务（计算、数据处理） |
| **内存开销** | 较小 | 较大 |
| **通信方式** | 共享内存，需同步机制 | 进程间通信（IPC） |

**GIL（全局解释器锁）**：Python解释器的一个机制，确保同一时刻只有一个线程执行Python字节码。这意味着多线程在CPU密集型任务中无法实现真正的并行。

**选择建议**：
- **I/O密集型任务**：使用多线程（网络请求、数据库操作、文件读写）。
- **CPU密集型任务**：使用多进程（数值计算、图像处理、数据分析）。
- **混合型任务**：可以使用`concurrent.futures`模块，它提供了统一的接口来管理线程池和进程池。

**示例**：

```python
import threading
import multiprocessing
import time
import requests

def cpu_bound_task(n):
    """CPU密集型任务：计算大量数据的平方和"""
    total = 0
    for i in range(n):
        total += i ** 2
    return total

def io_bound_task(url):
    """I/O密集型任务：发送HTTP请求"""
    try:
        response = requests.get(url, timeout=5)
        return f"{url}: {response.status_code}"
    except Exception as e:
        return f"{url}: Error - {str(e)}"

# ============ 多线程示例 ============
print("=== 多线程示例（I/O密集型）===\n")

urls = [
    "https://www.example.com",
    "https://www.google.com",
    "https://www.github.com",
    "https://www.python.org",
]

# 串行执行
start_time = time.time()
serial_results = [io_bound_task(url) for url in urls]
serial_time = time.time() - start_time
print(f"Serial execution time: {serial_time:.2f} seconds")

# 多线程执行
start_time = time.time()
threads = []
thread_results = []

def thread_task(url):
    result = io_bound_task(url)
    thread_results.append(result)

for url in urls:
    thread = threading.Thread(target=thread_task, args=(url,))
    threads.append(thread)
    thread.start()

for thread in threads:
    thread.join()

thread_time = time.time() - start_time
print(f"Multithreading time: {thread_time:.2f} seconds")
print(f"Speedup: {serial_time / thread_time:.2f}x\n")

# ============ 多进程示例 ============
print("=== 多进程示例（CPU密集型）===\n")

# 准备数据
data_sizes = [1000000, 2000000, 3000000, 4000000]

# 串行执行
start_time = time.time()
serial_results = [cpu_bound_task(n) for n in data_sizes]
serial_time = time.time() - start_time
print(f"Serial execution time: {serial_time:.2f} seconds")

# 多进程执行
start_time = time.time()
with multiprocessing.Pool(processes=4) as pool:
    process_results = pool.map(cpu_bound_task, data_sizes)
process_time = time.time() - start_time
print(f"Multiprocessing time: {process_time:.2f} seconds")
print(f"Speedup: {serial_time / process_time:.2f}x\n")

# ============ 使用concurrent.futures ============
print("=== 使用concurrent.futures ===\n")

from concurrent.futures import ThreadPoolExecutor, ProcessPoolExecutor

# 线程池（I/O密集型）
with ThreadPoolExecutor(max_workers=4) as executor:
    start_time = time.time()
    future_results = list(executor.map(io_bound_task, urls))
    executor_time = time.time() - start_time
    print(f"ThreadPoolExecutor time: {executor_time:.2f} seconds")

# 进程池（CPU密集型）
with ProcessPoolExecutor(max_workers=4) as executor:
    start_time = time.time()
    future_results = list(executor.map(cpu_bound_task, data_sizes))
    executor_time = time.time() - start_time
    print(f"ProcessPoolExecutor time: {executor_time:.2f} seconds")

print("\n=== 线程同步示例 ===\n")

# 线程锁示例
class Counter:
    def __init__(self):
        self.value = 0
        self.lock = threading.Lock()
    
    def increment(self):
        with self.lock:
            current = self.value
            # 模拟一些处理时间
            import time
            time.sleep(0.001)
            self.value = current + 1

def worker(counter):
    for _ in range(100):
        counter.increment()

# 测试线程安全
counter = Counter()
threads = []

for _ in range(5):
    thread = threading.Thread(target=worker, args=(counter,))
    threads.append(thread)
    thread.start()

for thread in threads:
    thread.join()

print(f"Final counter value: {counter.value}")
print(f"Expected value: {500}")
print(f"Thread-safe: {counter.value == 500}")
```

## 5. 常用模块与工具

### 5.1 常用标准库模块

**问题**：Python中有哪些常用的标准库模块？

**解析**：

Python标准库提供了大量实用的模块，以下是一些常用的模块：

| 模块 | 用途 |
|------|------|
| `os` | 操作系统接口，文件和目录操作 |
| `sys` | 系统相关的参数和函数 |
| `datetime` | 日期和时间处理 |
| `json` | JSON数据的编码和解码 |
| `re` | 正则表达式操作 |
| `collections` | 高级数据结构（Counter、deque、OrderedDict等） |
| `itertools` | 迭代器工具 |
| `functools` | 高阶函数和可调用对象操作 |
| `math` / `statistics` / `random` / `decimal` | 数学运算 |
| `urllib` / `http` | URL处理和HTTP协议 |
| `unittest` / `doctest` | 单元测试 |
| `logging` | 日志记录 |
| `argparse` | 命令行参数解析 |
| `pickle` / `shelve` | 对象序列化 |

**示例**：

```python
import os
import sys
import datetime
import json
import re
from collections import Counter, deque, defaultdict
import random
import logging

# ============ os模块 ============
print("=== os模块 ===")
print(f"当前工作目录: {os.getcwd()}")
print(f"环境变量PATH: {os.environ.get('PATH', 'Not found')[:50]}...")
print(f"操作系统类型: {os.name}")
print(f"路径分隔符: {os.sep}")

# 创建临时目录
temp_dir = os.path.join(os.getcwd(), "temp_test")
os.makedirs(temp_dir, exist_ok=True)
print(f"创建目录: {temp_dir}")

# 清理
os.rmdir(temp_dir)
print(f"删除目录: {temp_dir}")

# ============ sys模块 ============
print("\n=== sys模块 ===")
print(f"Python版本: {sys.version}")
print(f"平台: {sys.platform}")
print(f"命令行参数: {sys.argv}")
print(f"模块搜索路径数量: {len(sys.path)}")

# ============ datetime模块 ============
print("\n=== datetime模块 ===")
now = datetime.datetime.now()
print(f"当前时间: {now}")
print(f"格式化时间: {now.strftime('%Y-%m-%d %H:%M:%S')}")
print(f"日期: {now.date()}")
print(f"时间: {now.time()}")

# 时间差计算
future = now + datetime.timedelta(days=7)
print(f"一周后: {future.strftime('%Y-%m-%d')}")

# ============ json模块 ============
print("\n=== json模块 ===")
data = {
    "name": "Alice",
    "age": 25,
    "hobbies": ["reading", "swimming"],
    "address": {
        "city": "Beijing",
        "zipcode": "100000"
    }
}

# Python对象转JSON字符串
json_str = json.dumps(data, indent=2, ensure_ascii=False)
print(f"JSON字符串:\n{json_str}")

# JSON字符串转Python对象
parsed_data = json.loads(json_str)
print(f"解析后的数据: {parsed_data['name']}")

# 写入JSON文件
with open('temp_data.json', 'w', encoding='utf-8') as f:
    json.dump(data, f, indent=2, ensure_ascii=False)
print("数据已写入 temp_data.json")

# 删除临时文件
import os
os.remove('temp_data.json')

# ============ re模块 ============
print("\n=== re模块 ===")
text = "Hello, my email is test@example.com and my phone is 138-1234-5678"

# 查找邮箱
email_pattern = r'\b[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Z|a-z]{2,}\b'
email = re.search(email_pattern, text)
if email:
    print(f"找到邮箱: {email.group()}")

# 查找手机号
phone_pattern = r'\d{3}-\d{4}-\d{4}'
phone = re.search(phone_pattern, text)
if phone:
    print(f"找到手机号: {phone.group()}")

# 替换
new_text = re.sub(email_pattern, '[EMAIL]', text)
print(f"替换后: {new_text}")

# 分割
words = re.split(r'\s+', "Hello   world  Python")
print(f"分割结果: {words}")

# ============ collections模块 ============
print("\n=== collections模块 ===")

# Counter - 计数器
from collections import Counter
words = ['apple', 'banana', 'apple', 'orange', 'banana', 'apple']
word_count = Counter(words)
print(f"单词计数: {word_count}")
print(f"最常见的单词: {word_count.most_common(2)}")

# deque - 双端队列
from collections import deque
d = deque([1, 2, 3])
d.append(4)       # 右侧添加
d.appendleft(0) # 左侧添加
print(f"双端队列: {d}")
d.pop()         # 右侧移除
d.popleft()     # 左侧移除
print(f"移除后: {d}")

# defaultdict - 默认字典
from collections import defaultdict
dd = defaultdict(list)
dd['fruits'].append('apple')
dd['fruits'].append('banana')
dd['vegetables'].append('carrot')
print(f"默认字典: {dict(dd)}")

# namedtuple - 命名元组
from collections import namedtuple
Point = namedtuple('Point', ['x', 'y'])
p = Point(3, 4)
print(f"点坐标: ({p.x}, {p.y})")
print(f"距离原点: {math.sqrt(p.x**2 + p.y**2)}")

# OrderedDict - 有序字典（Python 3.7+ dict已默认有序）
from collections import OrderedDict
od = OrderedDict()
od['a'] = 1
od['b'] = 2
od['c'] = 3
print(f"有序字典: {od}")

# ============ random模块 ============
print("\n=== random模块 ===")

# 随机数
print(f"随机整数 (1-10): {random.randint(1, 10)}")
print(f"随机浮点数 (0-1): {random.random():.4f}")
print(f"随机范围 (1-100 步长5): {random.randrange(1, 100, 5)}")

# 随机选择
choices = ['apple', 'banana', 'orange', 'grape']
print(f"随机选择: {random.choice(choices)}")
print(f"随机选择多个: {random.sample(choices, 2)}")

# 打乱顺序
items = [1, 2, 3, 4, 5]
random.shuffle(items)
print(f"打乱后: {items}")

# 设置随机种子（可重现）
random.seed(42)
print(f"设置种子后的随机数: {random.random()}")

# ============ logging模块 ============
print("\n=== logging模块 ===")

# 配置日志
logging.basicConfig(
    level=logging.DEBUG,
    format='%(asctime)s - %(name)s - %(levelname)s - %(message)s',
    datefmt='%Y-%m-%d %H:%M:%S'
)

# 创建logger
logger = logging.getLogger('my_logger')

# 不同级别的日志
logger.debug('This is a debug message')
logger.info('This is an info message')
logger.warning('This is a warning message')
logger.error('This is an error message')
logger.critical('This is a critical message')

# 文件日志
file_handler = logging.FileHandler('app.log')
file_handler.setLevel(logging.INFO)
file_formatter = logging.Formatter('%(asctime)s - %(levelname)s - %(message)s')
file_handler.setFormatter(file_formatter)

file_logger = logging.getLogger('file_logger')
file_logger.addHandler(file_handler)
file_logger.info('This message goes to the file')

# 清理日志文件
import os
if os.path.exists('app.log'):
    os.remove('app.log')

print("\n所有模块示例已完成！")
```
## 其他常见面试题


### 1. 解释型和编译型语言的区别
- 编译型语言：把做好的源程序全部编译成二进制的可运行程序。然后，可直接运行这个程序。如：C，C++ ；
- 解释型语言：把做好的源程序翻译一句，然后执行一句，直至结束！如：Python。
注意：Java 有些特殊，java程序也需要编译，但是没有直接编译成为机器语言，而是编译称为字节码，然后用解释方式执行字节码。


### 2. 简述下 Python 中的字符串、列表、元组和字典
- 字符串（str）：字符串是用引号括起来的任意文本，是编程语言中最常用的数据类型。
- 列表（list）：列表是有序的集合，可以向其中添加或删除元素。
- 元组（tuple）：元组也是有序集合，元组中的数无法修改。即元组是不可变的。
- 字典（dict）：字典是无序的集合，是由键值对（key-value）组成的。
- 集合（set）：是一组 key 的集合，每个元素都是唯一，不重复且无序的。

### 3. 简述上述数据类型的常用方法
#### 字符串：
- 切片：'luobodazahui'[1:3]
- format："welcome to luobodazahui, dear {name}"format(name="baby")
- join：可以用来连接字符串，将字符串、元组、列表中的元素以指定的字符(分隔符)连接生成一个新的字符串。'-'.join(['luo', 'bo', 'da', 'za', 'hui'])
- String.replace(old,new,count)：将字符串中的 old字符替换为 New字符，count为替换的个数 'luobodazahui-haha'.replace('haha', 'good')
- split：切割字符串，得到一个列表

```
>>> mystr5 = 'luobo,dazahui good'

>>> print(mystr5.split())  # 默认以空格分割
['luobo,dazahui', 'good']

>>> print(mystr5.split('h'))  # 以h分割
['luobo,daza', 'ui good']

>>> print(mystr5.split(','))  # 以逗号分割
['luobo', 'dazahui good']

```
#### 列表：

- 切片，同字符串
- append和 extend向列表中添加元素
```
>>> mylist1 = [1, 2]
>>> mylist2 = [3, 4]
>>> mylist3 = [1, 2]

>>> mylist1.append(mylist2)
>>> print(mylist1)
[1, 2, [3, 4]]

>>> mylist3.extend(mylist2)
>>> print(mylist3)
[1, 2, 3, 4]
```

- 删除元素
del：根据下标进行删除
pop：删除最后一个元素
remove：根据元素的值进行删除
```
>>> mylist4 = ['a', 'b', 'c', 'd']

>>> del mylist4[0]
>>> print(mylist4)
['b', 'c', 'd']

>>> mylist4.pop()
>>> print(mylist4)
['b', 'c']

>>> mylist4.remove('c')
>>> print(mylist4)
['b']
```

- 元素排序 sort：是将list按特定顺序重新排列，默认为由小到大，参数 reverse=True可改为倒序，由大到小。
```
>>> mylist5 = [1, 5, 2, 3, 4]
>>> mylist5.sort()
>>> print(mylist5)
[1, 2, 3, 4, 5]
>>> mylist5.reverse()
>>> print(mylist5)
[5, 4, 3, 2, 1]
```
- reverse：是将list逆置。
#### 字典：

- 清空字典 dict.clear()
```
>>> dict1 = {'key1':1, 'key2':2}
>>> dict1.clear()
>>> dict1
{}
```
指定删除：使用 pop方法来指定删除字典中的某一项（随机的）。
```
>>> dict1 = {'key1':1, 'key2':2}
>>> d1 = dict1.pop('key1')
>>> dict1
{'key2': 2}
>>> d1
1
```

- 遍历字典
```
>>> dict2 = {'key1':1, 'key2':2}
>>> mykey = [key for key in dict2]  # ['key1', 'key2']
>>> mykey
['key1', 'key2']
>>> myvalue = [value for value in dict2.values()]
>>> myvalue
[1, 2]
>>> key_value = [(k, v) for k, v in dict2.items()]
>>> key_value
[('key1', 1), ('key2', 2)]
```

- fromkeys用于创建一个新字典，以序列中元素做字典的键，value为字典所有键对应的初始值。
```
>>> keys = ['zhangfei', 'guanyu', 'liubei', 'zhaoyun']
>>> dict.fromkeys(keys, 0)
{'zhangfei': 0, 'guanyu': 0, 'liubei': 0, 'zhaoyun': 0}
```

### 4. 简述 Python 中的字符串编码
计算机在最初的设计中，采用了8个比特（bit）作为一个字节（byte）的方式。一个字节能表示的最大的整数就是255，如果要表示更大的整数，就必须用更多的字节。最早，计算机只有 ASCII 编码，即只包含大小写英文字母、数字和一些符号，这些对于其他语言，如中文，日文显然是不够用的。后来又发明了Unicode，Unicode把所有语言都统一到一套编码里，这样就不会再有乱码问题了。当需要保存到硬盘或者需要传输的时候，就转换为UTF-8编码。UTF-8 是隶属于 Unicode 的可变长的编码方式。

在 Python 中，以 Unicode 方式编码的字符串，可以使用 encode() 方法来编码成指定的 bytes，也可以通过 decode()方法来把 bytes编码成字符串。

```
>>> "你好".encode('utf-8')
b'\xe4\xbd\xa0\xe5\xa5\xbd'
>>> b'\xe4\xb8\xad\xe6\x96\x87'.decode('utf-8')
"你好"
```

### 5. 一行代码实现数值交换
```
>>> a, b = 1, 2
>>> a, b = b, a
>>> print(a, b)
```

### 6. is 和 == 的区别
==是比较操作符，只是判断对象的值（value）是否一致，而 is 则判断的是对象之间的身份（内存地址）是否一致。对象的身份，可以通过 id() 方法来查看。
只有 id一致时，is比较才会返回 True，而当 value一致时，== 比较就会返回 True。

### 7. Python 函数中的参数类型
位置参数，默认参数，可变参数，关键字参数。

### 8. *arg 和 **kwarg 作用
允许我们在调用函数的时候传入多个实参
```
>>> def test(*arg, **kwarg):
...     if arg:
...         print("arg:", arg)
...     if kwarg:
...         print("kearg:", kwarg)
...
>>> test('ni', 'hao', key='world')
arg: ('ni', 'hao')
kearg: {'key': 'world'}
```
可以看出，*arg 会把位置参数转化为 tuple，**kwarg 会把关键字参数转化为 dict。

### 10.获取当前时间
```
>>> import time
>>> import datetime
>>> print(datetime.datetime.now())
2022-09-12 19:51:24.314335
>>> print(time.strftime('%Y-%m-%d %H:%M:%S'))
2022-09-12 19:51:24
```


### 11.  PEP8 规范
简单列举10条：

尽量以免单独使用小写字母’l’，大写字母’O’，以及大写字母’I’等容易混淆的字母。
函数命名使用全部小写的方式，可以使用下划线。
常量命名使用全部大写的方式，可以使用下划线。
使用 has或 is前缀命名布尔元素，如: is_connect = True; has_member = False。
不要在行尾加分号，也不要用分号将两条命令放在同一行。
不要使用反斜杠连接行。
方法定义之间空1行，顶级定义之间空两行。
如果一个类不继承自其它类，就显式的从object继承。
内部使用的类、方法或变量前，需加前缀_表明此为内部使用的。
要用断言来实现静态类型检测。


### 12. Python 的深浅拷贝
浅拷贝
```
>>> import copy
>>> list1 = [1, 2, 3, [1, 2]]
>>> list2 = copy.copy(list1)
>>> list2.append('a')
>>> list2[3].append('a')
>>> list1
[1, 2, 3, [1, 2, 'a']]
>>> list2
[1, 2, 3, [1, 2, 'a'], 'a']
```
浅拷贝只成功”独立“拷贝了列表的外层，而列表的内层列表，还是共享的。（划重点！！！）

深拷贝
```
>>> import copy
>>> list1 = [1, 2, 3, [1, 2]]
>>> list3 = copy.deepcopy(list1)
>>> list3.append('a')
>>> list3[3].append('a')
>>> list1
[1, 2, 3, [1, 2]]
>>> list3
[1, 2, 3, [1, 2, 'a'], 'a']
```
深拷贝使得两个列表完全独立开来，每一个列表的操作，都不会影响到另一个。


### 13.  [lambda x:i*x for i in range(4)]🧡🧡
这是一道非常经典的题目了。
```
>>> def num():
...     return [lambda x:i*x for i in range(4)]
...
>>> [m(1) for m in num()]
[3, 3, 3, 3]
```
why?

详细看链接：《Python面试题目：[lambda x: x*i for i in range(4)]》


### 14.  打印九九乘法表
```
>>> for i in range(1, 10):
...     for j in range(1, i + 1):
...         print(f"{i}*{j}={i * j}", end=" ")
...     print()
...
1*1=1
2*1=2 2*2=4
3*1=3 3*2=6 3*3=9
4*1=4 4*2=8 4*3=12 4*4=16
5*1=5 5*2=10 5*3=15 5*4=20 5*5=25
6*1=6 6*2=12 6*3=18 6*4=24 6*5=30 6*6=36
7*1=7 7*2=14 7*3=21 7*4=28 7*5=35 7*6=42 7*7=49
8*1=8 8*2=16 8*3=24 8*4=32 8*5=40 8*6=48 8*7=56 8*8=64
9*1=9 9*2=18 9*3=27 9*4=36 9*5=45 9*6=54 9*7=63 9*8=72 9*9=81
```
知识点：print函数默认是会换行的，其有一个默认参数 end。

### 15.  filter、map、reduce 的作用
filter 函数用于过滤序列，它接收一个函数和一个序列，把函数作用在序列的每个元素上，然后根据返回值是True还是False决定保留还是丢弃该元素。
```
>>> mylist = list(range(10))
>>> list(filter(lambda x: x % 2 == 1, mylist))
[1, 3, 5, 7, 9]
```
map函数传入一个函数和一个序列，并把函数作用到序列的每个元素上，返回一个可迭代对象。
```
>>> list(map(lambda x: x % 2, mylist))
[0, 1, 0, 1, 0, 1, 0, 1, 0, 1]
>>> list(map(lambda x: x * 2, mylist))
[0, 2, 4, 6, 8, 10, 12, 14, 16, 18]
```
reduce函数用于递归计算，同样需要传入一个函数和一个序列，并把函数和序列元素的计算结果与下一个元素进行计算。
reduce()将一个数据集合（链表，元组等）中的所有数据进行下列操作：用传给reduce 中的 function函数 （有两个参数）先对集合中的第 1、2 个元素进行操作，得到的结果再与第三个数据用 function 函数运算，最后得到一个结果。
```
>>> from functools import reduce
>>> reduce(lambda x, y: x + y, range(101))
5050
```
reduce()的应用：

一行代码搞定计算所有元素的积：
```
reduce(lambda x, y: x * y, [1, 2, 3, 4])
```
计算列表中所有元素的最大值：
```
reduce(lambda x, y: x if x > y else y, [1, 2, 3, 4])
```
有一个列表：[3, 5, 8, 1]对应的是3581的每一个数字，要从这个列表计算出原来的数，可以这样做：
```
reduce(lambda x, y: x * 10 + y, [3, 5, 8, 1])
```
可以看出，上面的三个函数与匿名函数相结合使用，可以写出强大简洁的代码。

### 16.  为什么不建议函数的默认参数传入可变对象
例如：
```
>>> def test(L=[]):
...     L.append('test')
...     print(L)
...
>>> test()
['test']
>>> test()
['test', 'test']
```
默认参数是一个列表，是可变对象[]，Python在函数定义的时候，默认参数 L 的值就被计算出来了，是[]，每次调用函数，如果 L 的值变了，那么下次调用时，默认参数的值就已经不再是[]了。

### 17.  面向对象中__new__ 和 __init__ 区别（🧡🧡）
__new__是在实例创建之前被调用的，因为它的任务就是创建实例然后返回该实例对象，是个静态方法。
__init__是当实例对象创建完成后被调用的，然后设置对象属性的一些初始值，通常用在初始化一个类实例的时候，是一个实例方法。
__new__至少要有一个参数cls，代表当前类，此参数在实例化时由 Python 解释器自动识别。
__new__必须要有返回值，返回实例化出来的实例，这点在自己实现__new__时要特别注意，可以 return 父类（通过 super(当前类名, cls)）__new__出来的实例，或者直接是 object的__new__出来的实例。
__init__有一个参数 self，就是这个__new__返回的实例，__init__在__new__的基础上可以完成一些其它初始化的动作，__init__不需要返回值。
如果__new__创建的是当前类的实例，会自动调用__init__函数，通过 return 语句里面调用的__new__函数的第一个参数是 **cls**** 来保证是当前类实例**，如果是其他类的类名，那么实际创建返回的就是其他类的实例，其实就不会调用当前类的__init__函数，也不会调用其他类的__init__函数。

### 18.  三元运算规则
看下面的例子：如果 a>b 成立 就输出 a-b 否则a+b。
```
>>> >>> a, b = 1, 2
>>> h = a - b if a > b else a + b
>>> h
3
```
### 19.  生成随机数
```
>>> import random
>>> random.random()
0.7571910055209727
>>> random.randint(1, 100)
23
>>> random.uniform(1, 5)
3.0640732831151687
```
用np.random也可以！

### 20. zip 函数用法
zip() 函数将可迭代的对象作为参数，将对象中对应的元素打包成一个元组，然后返回由这些元组组成的列表。
```
>>> list1 = ['zhangfei', 'guanyu', 'liubei', 'zhaoyun']
>>> list2 = [0, 3, 2, 4]
>>> list(zip(list1, list2))
[('zhangfei', 0), ('guanyu', 3), ('liubei', 2), ('zhaoyun', 4)]
```
### 20. range和xrange的区别
只有Python2中才有xrange()和range()，Python3 中的range()其实就是Python2中xrange()。

range([start,] stop[, step])，根据start与stop指定的范围以及step设定的步长，生成一个序列；
xrange() 生成一个生成器，可以很大的节约内存。

### 21. with方法打开文件的作用
打开文件在进行读写的时候可能会出现一些异常状况，如果按照常规的f.open()写法，我们需要 try，except，finally，做异常判断，并且文件最终不管遇到什么情况，都要执行f.close() 关闭文件，with方法帮我们实现了finally 中 f.close()。

with open("hello.txt", "a") as f:
    f.write("hello world!")

### 22.  字符串转列表
```
>>> s = "1,2,3,4,5,6,7,8,9"
>>> s.split(",")
['1', '2', '3', '4', '5', '6', '7', '8', '9']
```

### 23. 字符串转整数
```
>>> s = "1,2,3,4,5,6,7,8,9"
>>> list(map(lambda x: int(x), s.split(",")))
[1, 2, 3, 4, 5, 6, 7, 8, 9]
```


### 24.  删除列表中的重复值
利用集合去重：
```
>>> mylist = [1, 2, 3, 4, 5, 5, 6, 7, 4, 3]
>>> list(set(mylist))
[1, 2, 3, 4, 5, 6, 7]
```

### 25.  字符串单词统计
统计字符串中字母个数：
```
>>> from collections import Counter
>>> mystr = 'sdfsfsfsdfsd,were,hrhrgege.sdfwe!sfsdfs'
>>> Counter(mystr)
Counter({'s': 9,
         'd': 5,
         'f': 7,
         ',': 2,
         'w': 2,
         'e': 5,
         'r': 3,
         'h': 2,
         'g': 2,
         '.': 1,
         '!': 1})
```
统计字符串中单词个数
```
>>> mystr2 = "hello, Nice to meet you!"
>>> len(mystr2.split(" "))
5
```

### 26.  列表推导，求奇偶数
```
>>> [x for x in range(20) if x % 2 == 1]
[1, 3, 5, 7, 9, 11, 13, 15, 17, 19]
```

### 27.  一行代码展开列表
```
>>> list1 = [[1, 2], [3, 4], [5, 6]]
>>> [j for i in list1 for j in i]
[1, 2, 3, 4, 5, 6]
```

### 28.  实现二分法查找函数
二分查找算法也称折半查找，基本思想就是折半，对比大小后再折半查找，必须是有序序列才可以使用二分查找。

非递归算法：
```
def binary_search(data, item):
    left = 0
    right = len(data) - 1
    while left <= right:
        mid = (left + right) // 2
        if data[mid] == item:
            return True
        elif data[mid] < item:
            left = mid + 1
        else:
            right = mid - 1
    return False
```
递归算法：
```
def binary_search(data, item):
    n = len(data)
    if n > 0:
        mid = n // 2
        if data[mid] == item:
            return True
        elif data[mid] > item:
            return binary_search(data[:mid], item)
        else:
            return binary_search(data[mid + 1:], item)
    return False
```

### 29.  合并两个元组到字典
如：("zhangfei", "guanyu")，(66, 80) -> {'zhangfei': 66, 'guanyu': 80}。
```
>>> a = ("zhangfei", "guanyu")
>>> b = (66, 80)
>>> dict(zip(a, b))
{'zhangfei': 66, 'guanyu': 80}
```

### 30. 给出如下代码的输入，并简单解释
例子1：
```
>>> a = (1, 2, 3, [4, 5, 6, 7], 8)
>>> a[3] = 2
```
报错：TypeError: ‘tuple’ object does not support item assignment。
原因：元组不能修改！tuple 是不可变类型，不能改变 tuple 里的元素。

例子2：
```
>>> a = (1, 2, 3, [4, 5, 6, 7], 8)
>>> a[3][2] = 2
>>> a
(1, 2, 3, [4, 5, 2, 7], 8)
```
list 是可变类型，改变其元素是允许的。


### 31.  字典和 json 转换
```
dict1 = {'zhangfei':1, "liubei":2, "guanyu": 4, "zhaoyun":3}
myjson = json.dumps(dict1)    # 字典转JSON
mydict = json.loads(myjson)   # JSON转字典
```

### 32.  列表推导式、字典推导式和生成器
列表推导式：返回一个列表。
```
>>> td_list = [i for i in range(10)]
>>> td_list
[0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
>>> type(td_list)
list
```
生成器：
```
>>> ge_list = (i for i in range(20))
>>> ge_list
<generator object <genexpr> at 0x000001C3C127AAC8>
>>> type(ge_list)
generator
```

字典推导式：
```
>>> dic = {k: 2 for k in ["a", "b", "c", "d"]}
>>> dic
{'a': 2, 'b': 2, 'c': 2, 'd': 2}
>>> type(dic)
dict
```


### 33.  简述 read、readline、readlines 的区别
read() 读取整个文件
readline() 读取下一行,使用生成器方法
readlines() 读取整个文件到一个迭代器以供我们遍历

### 34.  打乱一个列表
```
>>> import random
>>> list = list(range(1, 10))
>>> random.shuffle(list)
>>> list
[3, 9, 1, 4, 6, 2, 8, 7, 5]
```

### 35.  反转字符串
```
>>> 'luobodazahui'[::-1]
'iuhazadoboul'
```

### 36.  单下划线和双下划线的作用
__foo__：一种约定，Python内部的名字，用来区别其他用户自定义的命名，以防冲突，就是例如__init__()，__del__()，__call__()些特殊方法。
_foo：一种约定，用来指定变量私有。不能用from module import * 导入，其他方面和公有变量一样访问。
__foo：这个有真正的意义：解析器用_classname__foo来代替这个名字，以区别和其他类相同的命名，它无法直接像公有成员一样随便访问，通过对象名._类名__xxx这样的方式可以访问。

### 37.  新式类和旧式类
在 python 里凡是继承了 object的类，都是新式类
Python3 里只有新式类
Python2 里面继承 object的是新式类，没有写父类的是经典类
经典类目前在 Python 里基本没有应用

### 38.  Python 面向对象中的继承有什么特点
同时支持单继承与多继承，当只有一个父类时为单继承，当存在多个父类时为多继承；
子类会继承父类所有的属性和方法，子类也可以覆盖父类同名的变量和方法；
在继承中基类的构造（__init__()）方法不会被自动调用，它需要在其派生类的构造中专门调用；
在调用基类的方法时，需要加上基类的类名前缀，且需要带上 self 参数变量。区别于在类中调用普通函数时并不需要带上 self 参数。

### 39.  super 函数的作用
super()函数是用于调用父类(超类)的一个方法
```
class A():
    def funcA(self):
        print("this is func A")
        
class B(A):
    def funcA_in_B(self):
        super(B, self).funcA()
        
    def funcC(self):
        print("this is func C")
```
```
>>> ins = B()
>>> ins.funcA_in_B()
this is func A
>>> ins.funcC()
this is func C
```
### 40. 类中的各种函数
主要分为实例方法、类方法和静态方法。

实例方法
定义：第一个参数必须是实例对象，该参数名一般约定为“self”，通过它来传递实例的属性和方法（也可以传类的属性和方法）。
调用：只能由实例对象调用。

类方法
定义：使用装饰器@classmethod。第一个参数必须是当前类对象，该参数名一般约定为“cls”，通过它来传递类的属性和方法（不能传实例的属性和方法）。
调用：实例对象和类对象都可以调用。

静态方法
定义：使用装饰器@staticmethod。参数随意，没有“self”和“cls”参数，但是方法体中不能使用类或实例的任何属性和方法。
调用：实例对象和类对象都可以调用。

静态方法是类中的函数，不需要实例。静态方法主要是用来存放逻辑性的代码，主要是一些逻辑属于类，但是和类本身没有交互。即在静态方法中，不会涉及到类中的方法和属性的操作。可以理解为将静态方法存在此类的名称空间中。

类方法是将类本身作为对象进行操作的方法。他和静态方法的区别在于：不管这个方式是从实例调用还是从类调用，它都用第一个参数把类传递过来。


#### 41. 如何判断是函数还是方法
与类和实例无绑定关系的 function都属于函数（function）
与类和实例有绑定关系的 function都属于方法（method）
普通函数：
```
def func1():
    pass
>>> print(func1)
<function func1 at 0x000001C3C12A4438>
```
类中的函数：
```
class People(object):
    def func2(self):
        pass
    
    @staticmethod
    def func3():
        pass
    
    @classmethod
    def func4(cls):
        pass
    
>>> people = People()

>>> print(people.func2)
<bound method People.func2 of <__main__.People object at 0x000001C3C12A6A48>>

>>> print(people.func3)
<function People.func3 at 0x000001C3C12A4A68>

>>> print(people.func4)
<bound method People.func4 of <class '__main__.People'>>
```

### 41.  isinstance 的作用以及与type()的区别
isinstance()函数来判断一个对象是否是一个已知的类型，类似type()。

区别：

type()不会认为子类是一种父类类型，不考虑继承关系；
isinstance()会认为子类是一种父类类型，考虑继承关系。
```
class A(object):
    pass

class B(A):
    pass

>>> a = A()
>>> b = B()

>>> print(isinstance(a, A))
True
>>> print(type(a) == A)
True

>>> print(isinstance(b, A))
True
>>> print(type(b) == A)
False
```

### 43. 单例模式与工厂模式
单例模式：主要目的是确保某一个类只有一个实例存在；
工厂模式：包涵一个超类，这个超类提供一个抽象化的接口来创建一个特定类型的对象，而不是决定哪个对象可以被创建。
单例模式：
这种模式涉及到一个单一的类，该类负责创建自己的对象，同时确保只有单个对象被创建。这个类提供了一种访问其唯一的对象的方式，可以直接访问，不需要实例化该类的对象。
意图：保证一个类仅有一个实例，并提供一个访问它的全局访问点。
主要解决：一个全局使用的类频繁地创建与销毁。
举例：操作一个文件，应该用一个唯一的实例去操作。

工厂模式：
工厂类模式设计的核心是：让“生产”和“产品”解耦。
工厂模式的主要解决的问题：将原来分布在各个地方的对象创建过程单独抽离出来，交给工厂类负责创建。其他地方想要使用对象直接找工厂（即调用工厂的方法）获取对象。
优点：

一个调用者想创建一个对象，只要知道其名称就可以了。
扩展性高，如果想增加一个产品，只要扩展一个工厂类就可以。
屏蔽产品的具体实现，调用者只关心产品的接口。

### 44.  查看目录下的所有文件
```
>>> import os
>>> print(os.listdir('.'))
```

### 45.  计算由1到5组成的互不重复的三位数
```
for i in range(1, 6):
    for j in range(1, 6):
        for k in range(1, 6):
            if (i != j) and i != k and j != k:
                print(f"{i}{j}{k}")
```

### 46.  去除字符串首尾空格
```
>>> "   hello world    ".strip()
'hello world'
```

### 47.  去除字符串中间的空格
方法一：
```
>>> "hello you are good".replace(" ", "")
'helloyouaregood'
```
方法二：
```
>>> "".join("hello you are good".split(" "))
'helloyouaregood'
```

### 48. 字符串格式化方式
方法一：使用 % 操作符
```
print("This is for %s" % "Python")
print("This is for %s, and %s" %("Python", "You"))
```
方法二：str.format（在 Python3 中，引入了这个新的字符串格式化方法）
```
print("This is my {}".format("chat"))
print("This is {name}, hope you can {do}".format(name="zhouluob", do="like"))
```
方法三：f-strings（在 Python3-6 中，引入了这个新的字符串格式化方法）
```
name = "luobodazahui"
print(f"hello {name}")
运行项目并下载源码
```
一个复杂些的例子：
```
def mytest(name, age):
    return f"hello {name}, you are {age} years old!"

>>> people = mytest("luobo", 20)
>>> print(people)
hello luobo, you are 20 years old!
```

### 49. 将"hello world"转换为首字母大写"Hello World"
title() 函数：
```
>>> str1 = "hello world"
>>> str1.title()
'Hello World'
```
不使用 title() 函数
```
>>> str1 = "hello world"
>>> " ".join(list(map(lambda x: x.capitalize(), str1.split(" "))))
'Hello World'

>>> " ".join(list(map(lambda x: x[0].upper() + x[1:], str1.split(" "))))
'Hello World'
```
### 50. 一行代码转换列表中的整数为字符串
如：[1, 2, 3] -> [“1”, “2”, “3”]
```
>>> list1 = [1, 2, 3]
>>> list(map(lambda x: str(x), list1))
['1', '2', '3']
```

### 51.  Python 中的反射
反射就是通过字符串的形式，导入模块；通过字符串的形式，去模块寻找指定函数，并执行。利用字符串的形式去对象（模块）中操作（查找/获取/删除/添加）成员，一种基于字符串的事件驱动！

简单理解就是用来判断某个字符串是什么，是变量还是方法。
例子：先定义一个类：
```
class NewClass(object):
    def __init__(self, name, male):
        self.name = name
        self.male = male
        
    def myname(self):
        print(f'My name is {self.name}')
        
    def mymale(self):
        print(f'I am a {self.male}')

>>> people = NewClass('luobo', 'boy')
>>> print(hasattr(people, 'name'))
True
>>> print(getattr(people, 'name'))
luobo
>>> setattr(people, 'male', 'girl')
>>> print(getattr(people, 'male'))
girl
```
getattr，hasattr，setattr，delattr 对模块的修改都在内存中进行，并不会影响文件中真实内容。


### 52.  metaclass 元类
类与实例：首先定义类以后，就可以根据这个类创建出实例，所以：先定义类，然后创建实例；
类与元类：先定义元类， 根据 metaclass 创建出类，所以：先定义 metaclass，然后创建类。
```
class MyMetaclass(type):
    def __new__(cls, class_name, class_parents, class_attr):
        class_attr['print'] = "this is my metaclass's subclass %s" %class_name
        return type.__new__(cls, class_name, class_parents, class_attr)

    class MyNewclass(object, metaclass=MyMetaclass):
    pass

>>> myinstance = MyNewclass()
>>> myinstance.print
"this is my metaclass's subclass MyNewclass"
```


### 53.  sort 和 sorted 的区别
不管是list.sort方法还是sorted函数，都有两个可选的关键字参数。

reverse：如果被设定为True，被排序的序列里的元素会以降序输出。这个参数的默认值是False。
key：一个只有一个参数的函数，这个函数会被用在序列里的每一个元素上，所产生的结果将是排序算法依赖的对比关键字。比如说，在对一些字符串排序时，可以用key=str.lower来实现忽略大小写的排序，或者是用key=len进行基于字符串长度的排序。这个参数的默认值是恒等函数，也就是默认用元素自己的值来排序。
可选参数key还可以在内置函数min()和max()中起作用。另外，还有些标准库里的函数也接受这个参数，像itertools.groupby()和heapq.nlargest()等。

sort()是可变对象列表（list）的方法。sort()会改变可变对象。

```
>>> list1 = [2, 1, 3]
>>> print(list1.sort())
None
>>> list1
[1, 2, 3]
>>> dict1.sort()
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AttributeError: 'dict' object has no attribute 'sort'
```
sorted()是产生一个新的对象。sorted(L)返回一个排序后的L，不改变原始的L，sorted()适用于任何可迭代容器。

```
>>> dict1 = {'test1':1, 'test2':2}
>>> list1 = [2, 1, 3]
>>> print(sorted(dict1))
['test1', 'test2']
>>> print(sorted(list1))
[1, 2, 3]
```

### 54. Python 中的 GIL
GIL 是 Python 的全局解释器锁，同一进程中假如有多个线程运行，一个线程在运行 Python 程序的时候会占用 Python 解释器（加了一把锁即 GIL），使该进程内的其他线程无法运行，等该线程运行完后其他线程才能运行。如果线程运行过程中遇到耗时操作，则解释器锁解开，使其他线程运行。所以在多线程中，线程的运行仍是有先后顺序的，并不是同时进行。


### 55. 产生8位随机密码
string 模块用法：
1、将大写的ASCII字符列表和数字组合起来

```
>>> string.ascii_uppercase + string.digits
'ABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789'
```

2、string.printable输出被视为可打印符号的 ASCII 字符组成的字符串。

```
>>> string.printable
'0123456789abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ!"#$%&\'()*+,-./:;<=>?@[\\]^_`{|}~ \t\n\r\x0b\x0c'
```
string.printable[:-7]去除被视为空白符号的 ASCII 字符组成的字符串。

使用python的random模块，使用其中的choice方法，从给定的字符序列中随机选择字符组合。

```
>>> import random
>>> import string
>>> "".join(random.choice(string.printable[:-7]) for i in range(8))
```
这就产生了8位随机密码。


### 56. 输出原始字符
python提供了输出原始字符串“r”的方法。

```
>>> print('hello\nworld')
hello
world
>>> print(b'hello\nworld')
b'hello\nworld'
>>> print(r'hello\nworld')
hello\nworld
```

### 57. 简述 any() 和 all() 方法
all：如果存在 0，None，False 返回 False，否则返回 True；
any：如果都是 0，None，False时，返回 False。

例子：

```
>>> all([1, 2, 3, 0])
False
>>> all([1, 2, 3])
True
>>> any([1, 2, 3])
True
>>> any([0, None, False])
False
```

### 58. 反转整数
首先判断是否是整数，再判断是否是一位数字，最后再判断是不是负数。

```
 def reverse_int(x):
     if not isinstance(x, int):
         return False
     if -10 < x < 10:
         return x
     tmp = str(x)
     if tmp[0] != '-':
         tmp = tmp[::-1]
         return int(tmp)
    else:
        tmp = tmp[1:][::-1]
        x = int(tmp)
        return -x
```

```
>>> reverse_int(-23837)
-73832
```


### 59. 函数式编程
函数式编程是一种抽象程度很高的编程范式，纯粹的函数式编程语言编写的函数没有变量，因此，任意一个函数，只要输入是确定的，输出就是确定的，这种纯函数称之为没有副作用。而允许使用变量的程序设计语言，由于函数内部的变量状态不确定，同样的输入，可能得到不同的输出，因此，这种函数是有副作用的。由于 Python 允许使用变量，因此，Python 不是纯函数式编程语言。

函数式编程的一个特点就是，允许把函数本身作为参数传入另一个函数，还允许返回一个函数！

函数作为返回值例子：

```
 def sum(*args):
     def inner_sum():
         tmp = 0
         for i in args:
             tmp += i
         return tmp
     return inner_sum

>>> mysum = sum(2, 4, 6)
>>> print(type(mysum))
<class 'function'>
>>> mysum()
12
```

### 60. 简述闭包
如果在一个内部函数里，对在外部作用域（但不是在全局作用域）的变量进行引用，那么内部函数就被认为是闭包（closure）。


闭包特点：

必须有一个内嵌函数；
内嵌函数必须引用外部函数中的变量；
外部函数的返回值必须是内嵌函数。

### 61. 简述装饰器
装饰器是一种特殊的闭包，就是在闭包的基础上传递了一个函数，然后覆盖原来函数的执行入口，以后调用这个函数的时候，就可以额外实现一些功能了。

一个打印 log 的例子：

```
 import time
 def log(func):
     def inner_log(*args, **kw):
         print("Call: {}".format(func.__name__))
         return func(*args, **kw)
     return inner_log
 
 
@log
def timer():
    print(time.time())

timer()
# Call: timer
# 1560171403.5128365
```
本质上，decorator就是一个返回函数的高阶函数。


### 62. 协程的优点
协程的优点：

最大优势就是协程极高的执行效率。因为子程序切换不是线程切换，而是由程序自身控制，因此，没有线程切换的开销，和多线程比，线程数量越多，协程的性能优势就越明显。
不需要多线程的锁机制，因为只有一个线程，也不存在同时写变量冲突，在协程中控制共享资源不加锁，只需要判断状态就好了，所以执行效率比多线程高很多。
因为协程是一个线程执行，那怎么利用多核CPU呢？最简单的方法是多进程+协程，既充分利用多核，又充分发挥协程的高效率，可获得极高的性能。

其他一些重要的点：

协程并没有增加线程数量，只是在线程的基础之上通过分时复用的方式运行多个协程，而且协程的切换在用户态完成，切换的代价比线程从用户态到内核态的代价小很多。
因此在协程调用阻塞IO操作的时候，操作系统会让线程进入阻塞状态，当前的协程和其它绑定在该线程之上的协程都会陷入阻塞而得不到调度，这往往是不能接受的。
因此在协程中不能调用导致线程阻塞的操作。也就是说，协程只有和异步IO结合起来，才能发挥最大的威力。
协程对计算密集型的任务没有太大的好处，计算密集型的任务本身不需要大量的线程切换，因为协程主要解决以往线程或者进程上下文切换的开销问题，所以协程主要对那些I/O密集型应用更好。
协程只有和异步IO结合起来才能发挥出最大的威力。


### 63. 实现斐波那契数列
斐波那契数列：又称黄金分割数列，指的是这样一个数列：1、1、2、3、5、8、13、21、34、……在数学上，斐波纳契数列以如下被以递归的方法定义：
F ( 1 ) = 1 , F ( 2 ) = 1 , F ( n ) = F ( n − 1 ) + F ( n − 2 ) ( n ≥ 2 , n ∈ N ∗ ) F(1)=1,F(2)=1, F(n)=F(n-1)+F(n-2)(n\geq2,n\in N*)F(1)=1,F(2)=1,F(n)=F(n−1)+F(n−2)(n≥2,n∈N∗)

生成器法：

```
def fib(n):
    if n == 0:
        return False
    if not isinstance(n, int) or (abs(n) != n):
        return False
    a, b = 0, 1
    while n:
        a, b = b, a + b
        n -= 1
        yield a
        
>>> fib(10)
<generator object fib at 0x000001AF2F2395C8>
>>> [i for i in fib(10)]
[1, 1, 2, 3, 5, 8, 13, 21, 34, 55]
```
递归法：

```
def fib(n):
    if n == 0:
        return False
    if not isinstance(n, int) or (n < 0):
        return False
    if n <= 1:
        return 1
    return fib(n - 1) + fib(n - 2)

>>> fib(10)
55
>>> [fib(i) for i in range(1, 11)]
[1, 1, 2, 3, 5, 8, 13, 21, 34, 55]
```
\quad

### 64. 正则切分字符串
```
>>> import re
>>> str1 = "hello world:luobo dazahui"
>>> re.split(r":| ", str1)
['hello', 'world', 'luobo', 'dazahui']
```

### 65. yield 用法
yield 是用来生成迭代器的语法，在函数中，如果包含了 yield，那么这个函数就是一个迭代器。当代码执行至 yield 时，就会中断代码执行，直到程序调用 next() 函数时，才会在上次 yield 的地方继续执行。

### 67. 冒泡排序
```
def Bubble(data, reversed):
    for i in range(len(data) - 1):
        for j in range(len(data) - i - 1):
            if data[j]  > data[j + 1]:
                data[j], data[j + 1] = data[j + 1], data[j]
    if reversed:
        data.reverse()
    return data

>>> Bubble(list1, True)
[11, 9, 8, 5, 3, 2]
>>> Bubble(list1, False)
[2, 3, 5, 8, 9, 11]
```

### 68. 快速排序
思想：首先任意选取一个数据（通常选用数组的第一个数）作为关键数据，然后将所有比它小的数都放到它前面，所有比它大的数都放到它后面，这个过程称为一趟快速排序，之后再递归排序两边的数据。
挑选基准值：从数列中挑出一个元素，称为"基准"（pivot）。
分割：重新排序数列，所有比基准值小的元素摆放在基准前面，所有比基准值大的元素摆在基准后面（与基准值相等的数可以到任何一边）。在这个分割结束之后，对基准值的排序就已经完成。
递归排序子序列：递归地将小于基准值元素的子序列和大于基准值元素的子序列排序。
```
def partition(arr, low, high):
    pivot = arr[low]
    while low < high:
        while low < high and arr[high] >= pivot:
            high -= 1
        arr[low], arr[high] = arr[high], arr[low]
        while low < high and arr[low] <= pivot:
            low += 1
        arr[low], arr[high] = arr[high], arr[low]
    return low


def quickSort(arr, low, high):
    if low < high:
        pi = partition(arr, low, high)
        quickSort(arr, low, pi - 1)
        quickSort(arr, pi + 1, high)
        

>>> list1 = [8, 5, 1, 3, 2, 10, 11, 4, 12, 20]
>>> quickSort(list1, 0, len(list1) - 1)
>>> list1
[1, 2, 3, 4, 5, 8, 10, 11, 12, 20]
```

### 70. requests 简介
该库是发起 HTTP 请求的强大类库，调用简单，功能强大。

```
>>> import requests
>>> url = "http://www.baidu.com"
>>> response = requests.get(url)  # 获得请求
>>> response.encoding = "utf-8"  # 改变其编码
>>> html = response.text  # 获得网页内容
>>> binary_content = response.content  # 获得二进制数据
>>> raw = requests.get(url, stream=True)  # 获得原始响应内容
>>> headers = {'user-agent': 'my-test/0.1.1'}  # 定制请求头
>>> r = requests.get(url, headers=headers)
>>> cookies = {"cookie": "# your cookie"}  # cookie的使用
>>> r = requests.get(url, cookies=cookies)
```

### 71. 比较两个 json 数据是否相等
方法一：
json数据转换成字典；
将两个字典按key排好序，然后使用zip()函数将两个字典对应的元素打包成元组。比较对应的元素的value是否相等；
zip()函数用于将可迭代的对象作为参数，将对象中对应的元素打包成一个个元组。

```
>>> file_text2 = '{"name":"john","age":22,"sex":"woman","address":"USA"}'
>>> file_text1 = '{"name":"john","age":22,"sex":"man","address":"USA"}'
>>> dict1 = json.loads(file_text1)
>>> dict2 = json.loads(file_text2)
>>> for s1, s2 in zip(sorted(dict1), sorted(dict2)):
        if str(dict1[s1]) != str(dict2[s2]):
            print(s1 + ":" + dict1[s1] + "!=" + s2 + ":" + dict2[s2])      
sex:man!=sex:woman
```
方法二：引入第三方模块jsonpatch。
```
>>> import jsonpatch
>>> src = {'numbers': [1, 3, 4, 8], 'foo': 'bar'}
>>> dst = {'foo': 'bar', 'numbers': [1, 3, 8]}
>>> patch = jsonpatch.JsonPatch.from_diff(src, dst)
>>> print(patch)
[{"op": "remove", "path": "/numbers/2"}]
```
说明：两个json对象，src要变成dst，需要移除numbers下索引是2的元素。
```
>>> src = {'numbers': [1, 3, 4, 8], 'foo': 'bar'}
>>> dst = {'foo': 'bar', 'numbers': [1, 3, 4, 8]}
>>> patch = jsonpatch.JsonPatch.from_diff(src, dst)
>>> print(patch)
[]
```
若相等的话直接返回空列表。

有了这个模块，判断json串是否相等不用遍历内部属性，直接判断patch长度即可。


### 72. 读取键盘输入
```
def forinput():
    input_text = input()
    print("your input text is: ", input_text)
    
    
>>> forinput()
>? 2
your input text is:  2
```

### 73. enumerate()的用法
enumerate()是 Python 内置函数。enumerate()函数用于将一个可遍历的数据对象（如列表、元组或字符串）组合为一个索引序列，同时列出数据和数据下标，一般用在for循环当中。

它允许我们遍历数据并自动计数，用法如下：

```
for counter, value in enumerate(some_list):
    print(counter, value)
```
不只如此，enumerate() 也接受一些可选参数，这使它更有用。

```
>>> my_list = ['apple', 'banana', 'grapes', 'pear']
>>> for c, value in enumerate(my_list, 1):
...     print(c, value)
...
1 apple
2 banana
3 grapes
4 pear
```
上面这个可选参数允许我们定制从哪个数字开始枚举。

此外，还可以用来创建包含索引的元组列表， 例如：
```
>>> my_list = ['apple', 'banana', 'grapes', 'pear']
>>> counter_list = list(enumerate(my_list, 1))
>>> print(counter_list)
[(1, 'apple'), (2, 'banana'), (3, 'grapes'), (4, 'pear')]
```

### 74. pass 语句
pass是空语句，是为了保持程序结构的完整性。pass不做任何事情，一般用做占位语句。
```
def forpass(n):
    if n == 1:
        pass
    else:
        print('not 1')
        
>>> forpass(1)
```


### 75. 正则匹配邮箱
```
>>> import re
>>> email_list= ["test01@163.com","test02@163.123", ".test03g@qq.com", "test04@gmail.com" ]
>>> for email in email_list:
        ret = re.match("[\w]{4,20}@(.*)\.com$", email)
        if ret:
            print("%s 是符合规定的邮件地址，匹配后结果是:%s" % (email, ret.group()))
        else:
            print("%s 不符合要求" % email)
        
test01@163.com 是符合规定的邮件地址，匹配后结果是:test01@163.com
test02@163.123 不符合要求
.test03g@qq.com 不符合要求
test04@gmail.com 是符合规定的邮件地址，匹配后结果是:test04@gmail.com
```


### 76. 统计字符串中大写字母的数量
```
>>> str2 = 'werrQWSDdiWuW'
>>> counter = 0
>>> for i in str2:
        if i.isupper():
            counter += 1  
>>> counter
6
```

### 77. json 序列化时保留中文
普通序列化：
```
>>> import json
>>> dict1 = {'name': '萝卜', 'age': 18}
>>> dict1_new = json.dumps(dict1)
>>> print(dict1_new)
{"name": "\u841d\u535c", "age": 18}
```

```
>>> import json
>>> dict1 = {'name': '萝卜', 'age': 18}
>>> dict1_new = json.dumps(dict1, ensure_ascii=False)
>>> print(dict1_new)
{"name": "萝卜", "age": 18}
```

### 78. 简述继承
一个类继承自另一个类，也可以说是一个孩子类/派生类/子类，继承自父类/基类/超类，同时获取所有的类成员（属性和方法）。继承使我们可以重用代码，并且还可以更方便地创建和维护代码。

Python 支持以下类型的继承：

单继承：一个子类类继承自单个基类
多重继承：一个子类继承自多个基类
多级继承：一个子类继承自一个基类，而基类继承自另一个基类
分层继承：多个子类继承自同一个基类
混合继承：两种或两种以上继承类型的组合

### 79. 什么是猴子补丁
猴子补丁是指在运行时动态修改类和模块。

猴子补丁主要有以下几个用处：

在运行时替换方法、属性等；
在不修改第三方代码的情况下增加原来不支持的功能；
在运行时为内存中的对象增加 patch 而不是在磁盘的源代码中增加。

### 80. help() 函数和 dir() 函数
help()函数返回帮助文档和参数说明
```
>>> help(dict)
Help on class dict in module builtins:
class dict(object)
 |  dict() -> new empty dictionary
 |  dict(mapping) -> new dictionary initialized from a mapping object's
 |      (key, value) pairs
 |  dict(iterable) -> new dictionary initialized as if via:
 |      d = {}
 |      for k, v in iterable:
 |          d[k] = v
 |  dict(**kwargs) -> new dictionary initialized with the name=value pairs
 |      in the keyword argument list.  For example:  dict(one=1, two=2)
 |  
 |  Methods defined here:
 ......
```

dir()函数返回对象中的所有成员 (任何类型)
```
>>> dir(dict)
['__class__',
 '__contains__',
 '__delattr__',
 '__delitem__',
 '__dir__',
 '__doc__',
 '__eq__',
 '__format__',
 '__ge__',
 '__getattribute__',
 ......
```

### 81. 解释 Python 中的//，％和**运算符
// 运算符执行地板除法，返回结果的整数部分 (向下取整)。
% 是取模符号，返回除法后的余数。
** 符号表示取幂。 a**b 返回 a 的b 次方。

### 82. 主动抛出异常
使用raise：
```
def test_raise(n):
    if not isinstance(n, int):
        raise Exception("not a int type")
    else:
        print("good")
        
>>> test_raise(8.9)
Traceback (most recent call last):
  File "D:\Anaconda3\lib\site-packages\IPython\core\interactiveshell.py", line 3331, in run_code
    exec(code_obj, self.user_global_ns, self.user_ns)
  File "<ipython-input-106-27962090d274>", line 1, in <module>
    test_raise(8.9)
  File "<ipython-input-105-8095f403c7e2>", line 3, in test_raise
    raise Exception("not a int type")
Exception: not a int type
```

### 83. tuple 和 list 转换
```
>>> tuple1 = (1, 2, 3, 4)
>>> list1 = list(tuple1)
>>> print(list1)
>>> tuple2 = tuple(list1)
>>> print(tuple2)
```

### 84. 简述断言
Python 的断言就是检测一个条件，如果条件为真，它什么都不做；反之它触发一个带可选错误信息的 AssertionError。
```
def testassert(n):
    assert n == 2, "n is not 2"
    print("n is 2")
    
>>> testassert(2)
n is 2
>>> testassert(3)
Traceback (most recent call last):
  File "D:\Anaconda3\lib\site-packages\IPython\core\interactiveshell.py", line 3331, in run_code
    exec(code_obj, self.user_global_ns, self.user_ns)
  File "<ipython-input-109-9b7ba30779d0>", line 1, in <module>
    testassert(3)
  File "<ipython-input-107-ecbc45f8f4b4>", line 2, in testassert
    assert n == 2, "n is not 2"
AssertionError: n is not 2
```

### 85. 什么是异步非阻塞
同步异步指的是调用者与被调用者之间的关系。

所谓同步，就是在发出一个功能调用时，在没有得到结果之前，该调用就不会返回，一旦调用返回，就得到了返回值；
异步的概念和同步相对，调用在发出之后，这个调用就直接返回了，所以没有返回结果。当该异步功能完成后，被调用者可以通过状态、通知或回调来通知调用者。
阻塞非阻塞是线程或进程之间的关系。

阻塞调用是指调用结果返回之前，当前线程会被挂起（如遇到io操作）。调用线程只有在得到结果之后才会返回。函数只有在得到结果之后才会将阻塞的线程激活
非阻塞和阻塞的概念相对应，非阻塞调用指在不能立刻得到结果之前也会立刻返回，同时该函数不会阻塞当前线程

### 86. 什么是负索引
Python 中的序列是有索引的，它由正数和负数组成。正的数字使用’0’作为第一个索引，‘1’作为第二个索引，以此类推。负数的索引从’-1’开始，表示序列中的最后一个索引，’ - 2’作为倒数第二个索引，依次类推。


### 87. 退出 Python 后，内存是否全部释放
不是的，那些具有对象循环引用或者全局命名空间引用的变量，在 Python 退出时往往不会被释放，
另外不会释放 C 库保留的部分内容。


### 88. Flask 和 Django 的异同
Flask 是 “microframework”，主要用来编写小型应用程序，不过随着 Python 的普及，很多大型程序也在使用 Flask。同时，在 Flask 中，我们必须使用外部库。
Django 适用于大型应用程序。它提供了灵活性，以及完整的程序框架和快速的项目生成方法。可以选择不同的数据库，URL结构，模板样式等。

### 89. 创建删除操作系统上的文件
```
>>> f = open('test.txt', 'w')
>>> f.close()
>>> os.listdir()
['.idea',
 'test.txt',
 '__pycache__']

>>> os.remove('test.txt')
>>> os.listdir()
['.idea',
 '__pycache__']
```

### 90. 简述 logging 模块
logging 模块是 Python 内置的标准模块，主要用于输出运行日志，可以设置输出日志的等级、日志保存路径、日志文件回滚等；相比 print，具备如下优点：

可以通过设置不同的日志等级，在 release 版本中只输出重要信息，而不必显示大量的调试信息
print 将所有信息都输出到标准输出中，严重影响开发者从标准输出中查看其它数据；logging 则可以由开发者决定将信息输出到什么地方，以及怎么输出。
简单配置：
```
>>> import logging
>>> logging.debug("debug log")
>>> logging.info("info log")
>>> logging.warning("warning log")
>>> logging.error("error log")
>>> logging.critical("critica log")
```
默认情况下，只显示了大于等于WARNING级别的日志。logging.basicConfig()函数调整日志级别、输出格式等。

### 91. 统计字符串中字符出现次数
```
>>> from collections import Counter
>>> str1 = "nihsasehndciswemeotpxc"
>>> print(Counter(str1))
Counter({'s': 3, 'e': 3, 'n': 2, 'i': 2, 'h': 2, 'c': 2, 'a': 1, 'd': 1, 'w': 1, 'm': 1, 'o': 1, 't': 1, 'p': 1, 'x': 1})
```

### 92. 正则 re.complie 的作用
re.compile 是将正则表达式编译成一个对象，加快速度，并重复使用。


### 93. try except else finally 的意义
try..except..else 没有捕获到异常，执行 else 语句
try..except..finally 不管是否捕获到异常，都执行 finally 语句

### 94. 反转列表
第一种方法：使用切片
```
>>> list1 = list(range(10))
>>> list1[::-1]
[9, 8, 7, 6, 5, 4, 3, 2, 1, 0]
```
第二种方法：使用reverse()
```
>>> list1 = list(range(10))
>>> list1.reverse()
>>> list1
[9, 8, 7, 6, 5, 4, 3, 2, 1, 0]
```
需要注意的是：这两种方法都可以反转列表，但内置函数 reverse() 会更改原始列表，而切片方法会创建一个新列表。内置函数 reverse() 比列表切片方法更快！


### 95. 字符串中数字替换
使用 re 正则替换：
```
>>> import re
>>> str1 = '我是周萝卜，今年18岁'
>>> re.sub(r"\d+", "20", str1)
'我是周萝卜，今年20岁'
```

### 96. 读取大文件
现要处理一个大小为10G的文件，但是内存只有4G，如果在只修改get_lines 函数而其他代码保持不
变的情况下，应该如何实现？需要考虑的问题都有那些？
```
def get_lines():
    with open('file.txt','rb') as f: 
        return f.readlines()
    
if name == ' main ': 
    for e in get_lines():
        process(e) # 处理每一行数据
```
方法一：readlines()函数在文件过大时并不适用，应添加参数，限制读取的字节数，并使用生成器。
```
def get_lines(): 
    l = []
    with open('file.txt','rb') as f: 
        data = f.readlines(60000)
    l.append(data) 
    yield l
```
方法二：使用mmap
```
from mmap import mmap

def get_lines(fp):
    with open(fp, "r+") as f:
        m = mmap(f.fileno(), 0) 
        tmp = 0
        for i, char in enumerate(m): 
            if char==b"\n":
                yield m[tmp:i + 1].decode() 
                tmp = i + 1

if name ==" main ":
    for i in get_lines("fp_some_huge_file"): 
        print(i)
```

### 97. 输入日期， 判断这一天是这一年的第几天
```
import datetime

def dayofyear():
    year = input("请输入年份: ")
    month = input("请输入月份: ")
    day = input("请输入天: ")
    date1 = datetime.date(year=int(year), month=int(month), day=int(day))
    date2 = datetime.date(year=int(year), month=1, day=1)
    return (date1 - date2).days + 1

>>> dayofyear()
请输入年份: >? 2022
请输入月份: >? 5
请输入天: >? 23
143
```

### 98. 排序
根据字典按值排序

现有字典d= {'a':24,'g':52,'i':12,'k':33}请按value值进行排序?
```
>>> d = {'a': 24, 'g': 52, 'i': 12, 'k': 33}
>>> d.items()
dict_items([('a', 24), ('g', 52), ('i', 12), ('k', 33)])

>>> sorted(d.items(), key=lambda x: x[1])
[('i', 12), ('a', 24), ('k', 33), ('g', 52)]
```
x[0]代表用key进行排序；x[1]代表用value进行排序。

请按alist中元素的age由大到小排序
```
>>> alist = [{'name': 'a', 'age': 20}, {'name': 'b', 'age': 30}, {'name': 'c', 'age': 25}]
>>> sorted(alist, key=lambda x:x['age'], reverse=True)
[{'name': 'b', 'age': 30}, {'name': 'c', 'age': 25}, {'name': 'a', 'age': 20}]
```
列表内，字典按照 value 大小排序
```
>>> list1 = [{'name': 'guanyu', 'age':29},
...         {'name': 'zhangfei', 'age': 28},
...         {'name': 'liubei', 'age':31}]
>>> sorted(list1, key=lambda x:x['age'])
[{'name': 'zhangfei', 'age': 28}, {'name': 'guanyu', 'age': 29}, {'name': 'liubei', 'age': 31}]
>>> sorted(list1, key=lambda x:x['name'])
[{'name': 'guanyu', 'age': 29}, {'name': 'liubei', 'age': 31}, {'name': 'zhangfei', 'age': 28}]
```

### 99. 将字符串处理成字典
```
>>> k = "k:1|k1:2|k2:3|k3:4"
>>> dict1 = {}
>>> for items in k.split("|"):
...    key, value = items.split(":")
...    dict1[key] = value
```

```
>>> {k:v for items in k.split("|") for k, v in (items.split(":"), )}
{'k': '1', 'k1': '2', 'k2': '3', 'k3': '4'}
```

### 100. 下面代码的输出结果将是什么？
```
list = ['a','b','c','d','e'] 
print(list[10:]) 
```
代码将输出[]，不会产生IndexError错误，就像所期望的那样，尝试用超出成员的个数的index来获取某个列表的成员。例如，尝试获取list[10]和之后的成员，会导致IndexError。然而，尝试获取列表的切片，开始的index超过了成员个数不会产生IndexError，而是仅仅返回一个空列表。这成为特别让人恶心的疑难杂症，因为运行的时候没有错误产生，导致Bug很难被追踪到。

这部分内容的参考文档是：https://blog.csdn.net/qq_37085158/article/details/126821933
## 总结

本文精选了Python面试中常见的问题，涵盖了Python基础、数据结构与算法、面向对象编程、并发编程、常用模块与工具等方面。每个问题都提供了详细的解析和代码示例，帮助读者理解和掌握Python的核心知识。

Python是一门简洁而强大的语言，拥有丰富的生态系统和活跃的社区。希望本文能够帮助读者在Python面试中取得好成绩，同时也能够提高读者的Python编程水平。

Good luck with your interviews! 🚀