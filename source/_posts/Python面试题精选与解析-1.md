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

**输出**：

```
Original: {'name': 'Alice', 'age': 25, 'hobbies': ['reading', 'swimming', 'coding']}
Shallow copy: {'name': 'Alice', 'age': 25, 'hobbies': ['reading', 'swimming', 'coding']}
Deep copy: {'name': 'Alice', 'age': 25, 'hobbies': ['reading', 'swimming']}

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

## 总结

本文精选了Python面试中常见的问题，涵盖了Python基础、数据结构与算法、面向对象编程、并发编程、常用模块与工具等方面。每个问题都提供了详细的解析和代码示例，帮助读者理解和掌握Python的核心知识。

Python是一门简洁而强大的语言，拥有丰富的生态系统和活跃的社区。希望本文能够帮助读者在Python面试中取得好成绩，同时也能够提高读者的Python编程水平。

Good luck with your interviews! 🚀