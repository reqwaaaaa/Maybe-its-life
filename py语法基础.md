# Python 基本操作与数据结构

## 一、Python 关键字和基础语法

### 常见关键字

- **import**：引入模块，例如 `import math`
- **def**：定义函数，例如 `def func_name():`
- **class**：定义类，例如 `class MyClass:`
- **if / elif / else**：条件判断语句
- **for / while**：循环语句
- **break / continue**：控制循环的中断和跳过
- **return**：函数返回值
- **yield**：生成器中返回值，暂停后继续执行
- **try / except / finally**：异常处理
- **lambda**：定义匿名函数，例如 `lambda x: x * 2`
- **with**：上下文管理，用于简化资源管理，如文件读写
- **pass**：占位符，表示“无操作”

---

## 二、基本结构

### 1. 变量定义

变量不需要声明类型，可以直接赋值：

```python
x = 10
name = "Alice"
is_active = True
```

### 2. 数据类型

- **整数**：`int`
- **浮点数**：`float`
- **字符串**：`str`
- **布尔**：`bool` (`True` 或 `False`)
- **列表**：`list`
- **字典**：`dict`
- **集合**：`set`
- **元组**：`tuple`

---

## 三、条件判断语句

**格式**：

```python
if condition1:
    # 执行代码块 1
elif condition2:
    # 执行代码块 2
else:
    # 执行代码块 3
```

**示例**：

```python
age = 20
if age >= 18:
    print("成人")
elif age >= 13:
    print("青少年")
else:
    print("儿童")
```

---

## 四、循环结构

### 1. `for` 循环

用于遍历序列或指定次数循环：

```python
for i in range(5):  # 输出 0 到 4
    print(i)

# 遍历列表
fruits = ["apple", "banana", "cherry"]
for fruit in fruits:
    print(fruit)
```

### 2. `while` 循环

只要条件为 `True`，不断循环：

```python
count = 0
while count < 5:
    print(count)
    count += 1
```

### 3. `break` 和 `continue`

- **`break`**：退出当前循环
- **`continue`**：跳过本次循环

```python
for i in range(5):
    if i == 3:
        break  # 当 i == 3 时退出循环
    print(i)

for i in range(5):
    if i == 3:
        continue  # 跳过 i == 3 的本次循环
    print(i)
```

---

## 五、Python 数据结构

### 1. 列表（List）

- **定义**：有序可变的数据结构，用 `[]` 表示。
- **操作**：`append()`、`insert()`、`remove()`、`pop()`、`index()`、`sort()` 等。

**示例**：

```python
# 定义和访问
fruits = ["apple", "banana", "cherry"]
print(fruits[0])  # 输出 'apple'

# 增加
fruits.append("orange")  # 末尾添加元素
fruits.insert(1, "blueberry")  # 指定位置插入

# 删除
fruits.remove("banana")  # 删除指定元素
last_fruit = fruits.pop()  # 删除并返回最后一个元素

# 修改
fruits[0] = "grape"  # 修改第一个元素
```

---

### 2. 元组（Tuple）

- **定义**：有序不可变的数据结构，用 `()` 表示。
- **操作**：`count()`、`index()`，因为元组不可变，所以没有增删操作。

**示例**：

```python
# 定义和访问
coordinates = (10, 20)
print(coordinates[0])  # 输出 10
```

---

### 3. 字典（Dictionary）

- **定义**：键值对的数据结构，用 `{}` 表示，键值对之间用逗号隔开。
- **操作**：`get()`、`keys()`、`values()`、`items()`、`pop()`、`update()`。

**示例**：

```python
# 定义和访问
student = {"name": "Alice", "age": 20, "major": "Math"}
print(student["name"])  # 输出 'Alice'

# 增加/更新
student["grade"] = "A"  # 增加新键值对
student["age"] = 21  # 更新 age 的值

# 删除
student.pop("major")  # 删除 'major' 键

# 获取键、值和键值对
keys = student.keys()
values = student.values()
items = student.items()
```

---

### 4. 集合（Set）

- **定义**：无序不重复的数据集合，用 `{}` 表示。
- **操作**：`add()`、`remove()`、`union()`、`intersection()`、`difference()` 等。

**示例**：

```python
# 定义和访问
unique_numbers = {1, 2, 3, 3}  # 重复的元素会被去掉
print(unique_numbers)  # 输出 {1, 2, 3}

# 增加
unique_numbers.add(4)

# 删除
unique_numbers.remove(2)

# 集合运算
set_a = {1, 2, 3}
set_b = {3, 4, 5}
print(set_a.union(set_b))  # 并集 {1, 2, 3, 4, 5}
print(set_a.intersection(set_b))  # 交集 {3}
print(set_a.difference(set_b))  # 差集 {1, 2}
```

---

## 六、函数定义与调用

**定义**：

```python
def function_name(parameter1, parameter2="default"):
    return parameter1 + parameter2
```

**调用**：

```python
result = function_name(5, 10)
print(result)  # 输出 15
```

### lambda 表达式

用于定义匿名函数，通常用于简短的计算。

**示例**：

```python
double = lambda x: x * 2
print(double(5))  # 输出 10
```

---

## 七、异常处理

用于捕获和处理程序中的异常，确保程序不因错误而中断。

**示例**：

```python
try:
    result = 10 / 0
except ZeroDivisionError:
    print("不能除以零")
finally:
    print("无论是否有错误都会执行")
```

---

## 八、文件操作

用于读写文件内容，常用 `with` 语句自动管理文件资源。

**示例**：

```python
# 写文件
with open("example.txt", "w") as file:
    file.write("Hello, world!")

# 读文件
with open("example.txt", "r") as file:
    content = file.read()
    print(content)
```

---

## 九、类与面向对象编程

**定义类**：

```python
class Animal:
    def __init__(self, name):
        self.name = name
    
    def speak(self):
        return f"{self.name} makes a sound"

# 创建实例
dog = Animal("Dog")
print(dog.speak())  # 输出 'Dog makes a sound'
```

### 继承

**示例**：

```python
class Dog(Animal):
    def speak(self):
        return f"{self.name} barks"

dog = Dog("Buddy")
print(dog.speak())  # 输出 'Buddy barks'
```

---

### 总结

- **Python 基础语法**：关键字、条件判断、循环。
- **数据结构操作**：列表、元组、字典、集合的增删改查。
- **函数和面向对象**：函数定义与调用、lambda 表达式、类与继承。
- **异常处理与文件操作**：try-except-finally 结构，文件读写。



