# Pandas 读取文件基础操作指南

这份笔记适合刚开始学习机器学习时查阅。机器学习第一步通常不是训练模型，而是把数据读进来、看懂它、检查它是否正常。

最常用的工具是 `pandas`，通常简写为 `pd`。

```python
import pandas as pd
```

## 1. 最常见：读取 CSV 文件

CSV 文件是机器学习练习里最常见的数据格式，文件后缀通常是 `.csv`。

```python
df = pd.read_csv("data.csv")
```

这里：

- `pd.read_csv(...)` 用来读取 CSV 文件。
- `"data.csv"` 是文件路径。
- `df` 是一个 DataFrame，可以理解为一张表格。

读取后可以直接查看前几行：

```python
df.head()
```

默认显示前 5 行。

如果想显示前 10 行：

```python
df.head(10)
```

## 2. 读取 TXT 文件

有些课程数据虽然是 `.txt` 后缀，但内容其实也是表格数据。

例如你的线性回归练习中可能会看到：

```python
data = pd.read_csv("ex1data1.txt", header=None, names=["Population", "Profit"])
```

这里的重点参数是：

- `header=None`：文件里没有表头。
- `names=[...]`：手动给每一列起名字。

如果文本文件用逗号分隔，`read_csv()` 可以直接读取。

如果文本文件用空格或制表符分隔，需要指定 `sep`。

## 3. 指定分隔符：`sep`

CSV 默认用逗号 `,` 分隔。

如果数据是用空格分隔：

```python
df = pd.read_csv("data.txt", sep=" ")
```

如果数据是用制表符分隔，也就是 tab：

```python
df = pd.read_csv("data.tsv", sep="\t")
```

如果数据中有多个空格，可以使用：

```python
df = pd.read_csv("data.txt", sep=r"\s+", engine="python")
```

其中：

- `\s+` 表示一个或多个空白字符。
- `engine="python"` 可以让 pandas 更灵活地解析这种分隔符。

## 4. 文件没有表头：`header=None`

如果文件第一行就是数据，而不是列名，需要写：

```python
df = pd.read_csv("data.csv", header=None)
```

否则 pandas 会把第一行数据误认为列名。

例如文件内容是：

```text
1,2
3,4
5,6
```

读取：

```python
df = pd.read_csv("data.csv", header=None)
```

结果会有默认列名：

```text
0, 1
```

## 5. 手动设置列名：`names`

如果文件没有表头，但你想给列起名字：

```python
df = pd.read_csv("data.csv", header=None, names=["x", "y"])
```

在线性回归练习中，常见写法是：

```python
data = pd.read_csv("ex1data1.txt", header=None, names=["Population", "Profit"])
```

这样后面就可以用列名取数据：

```python
X = data["Population"]
y = data["Profit"]
```

## 6. 读取 Excel 文件

Excel 文件通常是 `.xlsx` 或 `.xls`。

```python
df = pd.read_excel("data.xlsx")
```

如果一个 Excel 文件里有多个工作表，可以指定 `sheet_name`：

```python
df = pd.read_excel("data.xlsx", sheet_name="Sheet1")
```

也可以按编号读取，第一个工作表编号是 `0`：

```python
df = pd.read_excel("data.xlsx", sheet_name=0)
```

注意：读取 Excel 有时需要安装额外库，例如：

```bash
pip install openpyxl
```

## 7. 读取指定列：`usecols`

如果文件很大，只想读取其中几列：

```python
df = pd.read_csv("data.csv", usecols=["age", "income"])
```

也可以用列的位置：

```python
df = pd.read_csv("data.csv", usecols=[0, 2])
```

读取 Excel 时也可以用：

```python
df = pd.read_excel("data.xlsx", usecols=["A", "C"])
```

或者：

```python
df = pd.read_excel("data.xlsx", usecols="A:C")
```

## 8. 只读取前几行：`nrows`

如果文件很大，想先快速看看格式：

```python
df = pd.read_csv("data.csv", nrows=5)
```

这表示只读取前 5 行。

这在初次检查数据时很有用。

## 9. 跳过某些行：`skiprows`

如果文件开头有说明文字，不是真正的数据，可以跳过前几行：

```python
df = pd.read_csv("data.csv", skiprows=2)
```

这表示跳过前 2 行。

如果要跳过指定行：

```python
df = pd.read_csv("data.csv", skiprows=[0, 2])
```

这表示跳过第 0 行和第 2 行。

## 10. 编码问题：`encoding`

如果读取中文 CSV 时出现乱码或报错，可能是编码问题。

常见尝试：

```python
df = pd.read_csv("data.csv", encoding="utf-8")
```

如果不行，可以尝试：

```python
df = pd.read_csv("data.csv", encoding="gbk")
```

或者：

```python
df = pd.read_csv("data.csv", encoding="utf-8-sig")
```

常见情况：

- `utf-8`：最常见的现代编码。
- `utf-8-sig`：有些 Excel 导出的 CSV 会用这个。
- `gbk`：中文 Windows 环境里比较常见。

## 11. 缺失值处理：`na_values`

有些文件里会用特殊符号表示缺失值，例如：

```text
NA
?
missing
```

可以读取时告诉 pandas：

```python
df = pd.read_csv("data.csv", na_values=["NA", "?", "missing"])
```

读取后，这些值会被识别成缺失值 `NaN`。

检查缺失值：

```python
df.isnull().sum()
```

## 12. 读取后快速检查数据

读取文件后，不要急着训练模型。先检查数据是否正常。

### 查看前几行

```python
df.head()
```

### 查看后几行

```python
df.tail()
```

### 查看形状

```python
df.shape
```

输出类似：

```text
(100, 5)
```

表示：

```text
100 行，5 列
```

### 查看列名

```python
df.columns
```

### 查看每列数据类型

```python
df.dtypes
```

### 查看整体信息

```python
df.info()
```

### 查看数值列的统计信息

```python
df.describe()
```

## 13. 选择列和行

读取数据后，经常要选出特征 `X` 和目标值 `y`。

### 选择一列

```python
y = df["Profit"]
```

### 选择多列

```python
X = df[["Population", "Income"]]
```

注意：选择多列时要用双层中括号。

### 按位置选择列

```python
X = df.iloc[:, 0]
```

表示选择所有行的第 0 列。

```python
X = df.iloc[:, :-1]
y = df.iloc[:, -1]
```

常见含义：

- `df.iloc[:, :-1]`：选择除了最后一列以外的所有列，通常作为特征。
- `df.iloc[:, -1]`：选择最后一列，通常作为目标值。

## 14. 路径问题

读取文件时最常见的问题是路径写错。

### 当前目录下的文件

```python
df = pd.read_csv("data.csv")
```

表示 `data.csv` 和当前 notebook 或 Python 文件在同一个目录。

### 子文件夹里的文件

```python
df = pd.read_csv("data/data.csv")
```

表示文件在 `data` 文件夹里。

### Windows 绝对路径

Windows 路径里有反斜杠 `\`，容易和转义字符冲突。推荐用下面几种写法。

写法 1：使用原始字符串：

```python
df = pd.read_csv(r"C:\Users\Basin\data.csv")
```

写法 2：使用双反斜杠：

```python
df = pd.read_csv("C:\\Users\\Basin\\data.csv")
```

写法 3：使用正斜杠：

```python
df = pd.read_csv("C:/Users/Basin/data.csv")
```

初学时推荐第 1 种或第 3 种。

## 15. 查看当前工作目录

如果不知道 Python 当前在哪里找文件，可以用：

```python
import os

os.getcwd()
```

如果想查看当前目录下有哪些文件：

```python
os.listdir()
```

如果文件不在当前目录，就需要写正确的相对路径或绝对路径。

## 16. 常见读取函数小抄

```python
pd.read_csv("data.csv")                  # 读取 CSV 文件
pd.read_csv("data.txt")                  # 读取逗号分隔的 txt 文件
pd.read_csv("data.tsv", sep="\t")        # 读取 tab 分隔文件
pd.read_excel("data.xlsx")               # 读取 Excel 文件
pd.read_json("data.json")                # 读取 JSON 文件
pd.read_html("https://example.com")      # 读取网页表格
```

读完后常用检查：

```python
df.head()          # 前 5 行
df.tail()          # 后 5 行
df.shape           # 行数和列数
df.columns         # 列名
df.dtypes          # 数据类型
df.info()          # 整体信息
df.describe()      # 数值统计
df.isnull().sum()  # 每列缺失值数量
```

## 17. 一个完整例子

```python
import pandas as pd

data = pd.read_csv(
    "ex1data1.txt",
    header=None,
    names=["Population", "Profit"]
)

print(data.head())
print(data.shape)
print(data.info())

X = data["Population"]
y = data["Profit"]
```

如果后面要画图：

```python
import matplotlib.pyplot as plt

plt.scatter(X, y, marker="x", color="red")
plt.xlabel("Population")
plt.ylabel("Profit")
plt.title("Training Data")
plt.show()
```

## 18. 初学者建议

每次读取一个新文件后，先按这个顺序检查：

```python
df.head()
df.shape
df.columns
df.info()
df.describe()
df.isnull().sum()
```

最重要的是确认：

- 数据有没有读错行。
- 列名是否正确。
- 数值列有没有被读成字符串。
- 是否有缺失值。
- 特征 `X` 和目标值 `y` 是否选对。

