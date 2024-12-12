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

## 利用py画表

### 1. **导入库**

```python
import pandas as pd
import matplotlib.pyplot as plt
from matplotlib import rcParams
```

- **`pandas`**: 用于创建和操作数据表（DataFrame）。
- **`matplotlib.pyplot`**: 用于绘制图形和表格。
- **`rcParams`**: 用于调整 Matplotlib 的全局参数，如图像 DPI 和字体设置。


### 2. **字体与符号设置**

```python
plt.rcParams["font.sans-serif"] = ['SimHei']
plt.rcParams["axes.unicode_minus"] = False
rcParams.update({'figure.dpi': 150})
```

- **`plt.rcParams["font.sans-serif"] = ['SimHei']`**:
  - 设置 Matplotlib 使用中文字体（黑体）。否则中文可能显示为乱码。
  
- **`plt.rcParams["axes.unicode_minus"] = False`**:
  - 防止负号（`-`）在图表中显示为方块或乱码。

- **`rcParams.update({'figure.dpi': 150})`**:
  - 设置图像的 DPI（每英寸点数）。值越高，图像越清晰。


### 3. **表格数据定义**

```python
data = {
    "信息类别": [...],
    "具体内容": [...],
    "示例": [...]
}
```

- 定义一个字典 `data`，包含表格的内容，每列对应一个键。
- 每列数据的含义：
  - **`信息类别`**：描述数据的类型或维度。
  - **`具体内容`**：具体的数据公式或解释，使用 LaTeX 语法（如 `$n_1 \\\\to n_2 \\\\to \\\\dots$`）。
  - **`示例`**：提供具体的例子，帮助理解数据类别和内容。


#### 4. **创建 DataFrame**

```python
df = pd.DataFrame(data)
```

- 将字典 `data` 转换为 `pandas` 的 DataFrame，方便后续操作。


### 5. **绘制表格**

```python
fig, ax = plt.subplots(figsize=(12, 6))
ax.axis('tight')
ax.axis('off')
```

- **`plt.subplots(figsize=(12, 6))`**:
  - 创建一个绘图对象 `fig` 和一个轴对象 `ax`。
  - 设置图像的大小为宽 12 英寸、高 6 英寸。

- **`ax.axis('tight')`**:
  - 自动调整图像内容以适应轴范围。

- **`ax.axis('off')`**:
  - 隐藏坐标轴。


### 6. **绘制表格内容**

```python
table = ax.table(
    cellText=df.values,
    colLabels=df.columns,
    cellLoc='center',
    loc='center'
)
```

- **`cellText=df.values`**: 指定表格内容为 DataFrame 的值。
- **`colLabels=df.columns`**: 指定表头为 DataFrame 的列名。
- **`cellLoc='center'`**: 表格中的文本居中对齐。
- **`loc='center'`**: 表格在图像中居中显示。


### 7. **自动调整字体和列宽**

```python
table.auto_set_font_size(False)
table.set_fontsize(10)
table.auto_set_column_width(col=list(range(len(df.columns))))
```

- **`table.auto_set_font_size(False)`**: 禁止自动调整字体大小。
- **`table.set_fontsize(10)`**: 手动将字体大小设置为 10。
- **`table.auto_set_column_width(col=list(range(len(df.columns))))`**:
  - 自动调整每列的宽度以适应内容。
  - 参数 `col=list(range(len(df.columns)))` 确保调整所有列。


### 8. **调整行高**

```python
for key, cell in table.get_celld().items():
    cell.set_height(0.1)
```

- 遍历表格的所有单元格，设置每个单元格的高度为 0.1。
- **`table.get_celld()`**: 获取表格单元格的字典，每个单元格用 `(row, column)` 坐标标识。


### 9. **保存图片**

```python
output_path = "trajectory_hotspot_table_no_whitespace.png"
plt.savefig(output_path, bbox_inches='tight', dpi=300)
plt.close(fig)
```

- **`plt.savefig(output_path, bbox_inches='tight', dpi=300)`**:
  - 将图像保存为文件。
  - **`output_path`**: 输出文件路径。
  - **`bbox_inches='tight'`**: 去除多余留白。
  - **`dpi=300`**: 高分辨率保存。
  
- **`plt.close(fig)`**: 关闭当前图像对象，释放资源。


### 10. **打印保存信息**

```python
print(f"图片保存为 {output_path}")
```

- 使用 `f-string` 打印保存路径，确认操作成功。

---






