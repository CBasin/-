# `matplotlib.pyplot` 常用函数入门笔记

这份笔记适合刚开始学习机器学习和数据可视化时查阅。`matplotlib.pyplot` 通常会简写成 `plt`，它最常用于画折线图、散点图、柱状图、直方图，以及给图像添加标题、坐标轴、图例和网格。

## 1. 基本导入

```python
import matplotlib.pyplot as plt
import numpy as np
```

如果在 Jupyter Notebook 里画图，通常还会看到：

```python
%matplotlib inline
```

这行代码的作用是让图像直接显示在 notebook 单元格下面。

## 2. 最简单的折线图：`plt.plot()`

```python
x = [1, 2, 3, 4]
y = [2, 4, 6, 8]

plt.plot(x, y)
plt.show()
```

`plt.plot(x, y)` 会根据 `x` 和 `y` 画折线图。

如果只传入一个列表：

```python
plt.plot([2, 4, 6, 8])
plt.show()
```

Matplotlib 会自动把横坐标当成：

```text
0, 1, 2, 3
```

## 3. 添加标题和坐标轴标签

```python
plt.plot(x, y)
plt.title("Simple Line Plot")
plt.xlabel("x")
plt.ylabel("y")
plt.show()
```

常用函数：

```python
plt.title("标题")
plt.xlabel("横坐标名称")
plt.ylabel("纵坐标名称")
```

在机器学习里，画损失函数变化时常见写法：

```python
plt.plot(cost_history)
plt.xlabel("Iterations")
plt.ylabel("Cost")
plt.title("Cost During Training")
plt.show()
```

## 4. 设置线条样式、颜色和标记

```python
plt.plot(x, y, color="red", linestyle="--", marker="o")
plt.show()
```

常用参数：

- `color`：线条颜色。
- `linestyle`：线条样式。
- `marker`：每个数据点的标记。

常见颜色：

```python
"red", "blue", "green", "black", "orange"
```

常见线条样式：

```python
"-"   # 实线
"--"  # 虚线
":"   # 点线
"-."  # 点划线
```

常见标记：

```python
"o"  # 圆点
"x"  # 叉号
"s"  # 方块
"^"  # 三角形
```

也可以使用简写：

```python
plt.plot(x, y, "ro--")
plt.show()
```

其中：

```text
r  表示 red
o  表示圆点 marker
-- 表示虚线
```

## 5. 画多条线和图例：`plt.legend()`

```python
x = np.array([1, 2, 3, 4])

plt.plot(x, x, label="y = x")
plt.plot(x, x ** 2, label="y = x^2")
plt.plot(x, x ** 3, label="y = x^3")

plt.legend()
plt.show()
```

`label` 给每条线起名字，`plt.legend()` 显示图例。

也可以指定图例位置：

```python
plt.legend(loc="upper left")
```

常见位置：

```python
"upper left"
"upper right"
"lower left"
"lower right"
"best"
```

## 6. 散点图：`plt.scatter()`

散点图经常用于观察两个变量之间的关系。

```python
x = [1, 2, 3, 4, 5]
y = [2, 3, 5, 7, 11]

plt.scatter(x, y)
plt.xlabel("x")
plt.ylabel("y")
plt.show()
```

在线性回归里，散点图常用于画训练数据：

```python
plt.scatter(X, y, marker="x", color="red")
plt.xlabel("Population")
plt.ylabel("Profit")
plt.show()
```

常用参数：

```python
plt.scatter(x, y, color="red", marker="x", s=40)
```

- `color`：点的颜色。
- `marker`：点的形状。
- `s`：点的大小。

## 7. 柱状图：`plt.bar()`

柱状图适合比较不同类别的数值。

```python
names = ["A", "B", "C"]
values = [10, 20, 15]

plt.bar(names, values)
plt.xlabel("Category")
plt.ylabel("Value")
plt.show()
```

设置颜色：

```python
plt.bar(names, values, color="skyblue")
plt.show()
```

水平柱状图：

```python
plt.barh(names, values)
plt.show()
```

## 8. 直方图：`plt.hist()`

直方图适合观察一组数据的分布。

```python
data = np.random.randn(1000)

plt.hist(data, bins=30)
plt.xlabel("Value")
plt.ylabel("Frequency")
plt.show()
```

常用参数：

- `bins`：分成多少个区间。
- `color`：柱子颜色。
- `edgecolor`：柱子边框颜色。

```python
plt.hist(data, bins=30, color="lightblue", edgecolor="black")
plt.show()
```

## 9. 设置坐标轴范围

```python
plt.plot(x, y)
plt.xlim(0, 10)
plt.ylim(0, 20)
plt.show()
```

常用函数：

```python
plt.xlim(最小值, 最大值)
plt.ylim(最小值, 最大值)
```

这在你想放大观察某个区域时很有用。

## 10. 网格线：`plt.grid()`

```python
plt.plot(x, y)
plt.grid(True)
plt.show()
```

可以设置网格样式：

```python
plt.grid(True, linestyle="--", alpha=0.5)
```

其中：

- `linestyle` 控制网格线样式。
- `alpha` 控制透明度，范围通常是 `0` 到 `1`。

## 11. 图像大小：`plt.figure()`

```python
plt.figure(figsize=(8, 5))
plt.plot(x, y)
plt.show()
```

`figsize=(8, 5)` 表示图像宽 8 英寸，高 5 英寸。

通常写在画图之前：

```python
plt.figure(figsize=(10, 6))
plt.scatter(x, y)
plt.show()
```

## 12. 多个子图：`plt.subplot()`

如果想在一张图里放多个小图，可以使用 `subplot`。

```python
x = np.linspace(0, 10, 100)

plt.subplot(1, 2, 1)
plt.plot(x, np.sin(x))
plt.title("sin(x)")

plt.subplot(1, 2, 2)
plt.plot(x, np.cos(x))
plt.title("cos(x)")

plt.show()
```

`plt.subplot(1, 2, 1)` 的意思是：

```text
把画布分成 1 行 2 列，现在画第 1 个位置。
```

`plt.subplot(1, 2, 2)` 的意思是：

```text
把画布分成 1 行 2 列，现在画第 2 个位置。
```

也可以写成三位数简写：

```python
plt.subplot(121)
plt.subplot(122)
```

## 13. 更推荐的子图写法：`plt.subplots()`

`plt.subplots()` 更适合复杂图像，因为它会返回 `fig` 和 `ax`。

```python
fig, ax = plt.subplots()

ax.plot(x, y)
ax.set_title("Line Plot")
ax.set_xlabel("x")
ax.set_ylabel("y")

plt.show()
```

如果有多个子图：

```python
fig, axes = plt.subplots(1, 2, figsize=(10, 4))

axes[0].plot(x, np.sin(x))
axes[0].set_title("sin(x)")

axes[1].plot(x, np.cos(x))
axes[1].set_title("cos(x)")

plt.show()
```

初学时可以先用 `plt.plot()`、`plt.scatter()` 这类简单写法；以后图变复杂时，再多用 `fig, ax = plt.subplots()`。

## 14. 保存图片：`plt.savefig()`

```python
plt.plot(x, y)
plt.title("My Figure")
plt.savefig("my_figure.png")
plt.show()
```

常见保存格式：

```python
"figure.png"
"figure.jpg"
"figure.pdf"
"figure.svg"
```

如果想让图片更清晰，可以设置 `dpi`：

```python
plt.savefig("my_figure.png", dpi=300)
```

注意：通常把 `plt.savefig()` 放在 `plt.show()` 前面。

## 15. 清空图像：`plt.clf()` 和 `plt.close()`

在 notebook 或循环画图时，有时需要清空之前的图。

```python
plt.clf()
```

`plt.clf()` 清空当前 figure。

```python
plt.close()
```

`plt.close()` 关闭当前 figure。

如果循环保存很多图片，可以这样写：

```python
for i in range(3):
    plt.figure()
    plt.plot([1, 2, 3], [i, i + 1, i + 2])
    plt.savefig(f"figure_{i}.png")
    plt.close()
```

这样可以避免图像互相影响。

## 16. 机器学习里最常见的画图场景

### 画训练数据

```python
plt.scatter(X, y, marker="x", color="red")
plt.xlabel("x")
plt.ylabel("y")
plt.title("Training Data")
plt.show()
```

### 画拟合直线

```python
plt.scatter(X, y, marker="x", color="red", label="Training data")
plt.plot(X, predictions, color="blue", label="Prediction")
plt.xlabel("x")
plt.ylabel("y")
plt.legend()
plt.show()
```

### 画损失函数下降过程

```python
plt.plot(cost_history)
plt.xlabel("Iteration")
plt.ylabel("Cost")
plt.title("Cost Function")
plt.grid(True)
plt.show()
```

如果损失函数正常下降，通常会看到一条逐渐降低的曲线。

## 17. 常见报错和排查

### `x` 和 `y` 长度不一致

如果写：

```python
plt.plot([1, 2, 3], [1, 2])
```

会报错，因为横坐标有 3 个点，纵坐标只有 2 个点。

排查方式：

```python
print(len(x))
print(len(y))
```

如果是 NumPy 数组：

```python
print(x.shape)
print(y.shape)
```

### 图没有显示

在普通 `.py` 文件里，需要：

```python
plt.show()
```

在 Jupyter Notebook 里，通常可以加：

```python
%matplotlib inline
```

### 中文标题显示乱码

Matplotlib 默认字体可能不支持中文。可以尝试：

```python
plt.rcParams["font.sans-serif"] = ["SimHei"]
plt.rcParams["axes.unicode_minus"] = False
```

第一行设置中文字体，第二行避免负号显示异常。

## 18. 小抄

```python
import matplotlib.pyplot as plt

plt.figure(figsize=(8, 5))       # 设置图像大小
plt.plot(x, y)                   # 折线图
plt.scatter(x, y)                # 散点图
plt.bar(names, values)           # 柱状图
plt.barh(names, values)          # 水平柱状图
plt.hist(data, bins=30)          # 直方图

plt.title("Title")               # 标题
plt.xlabel("x label")            # 横坐标标签
plt.ylabel("y label")            # 纵坐标标签
plt.legend()                     # 图例
plt.grid(True)                   # 网格
plt.xlim(0, 10)                  # 横坐标范围
plt.ylim(0, 20)                  # 纵坐标范围

plt.savefig("figure.png", dpi=300) # 保存图片
plt.show()                         # 显示图片
```

最重要的入门习惯：

```python
print(x.shape)
print(y.shape)
```

很多画图问题不是 `matplotlib` 函数写错了，而是 `x`、`y` 的长度或形状没有对齐。

