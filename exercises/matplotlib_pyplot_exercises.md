# `matplotlib.pyplot` 绘制图表练习题

这份练习配合 [`matplotlib.pyplot` 常用函数入门笔记](../notes/matplotlib_pyplot_notes.md) 使用。建议在 Jupyter Notebook 里完成，每道题都先自己写代码，再对照提示检查。

开始前先导入：

```python
import numpy as np
import matplotlib.pyplot as plt
```

如果你在 Jupyter Notebook 中，可以加上：

```python
%matplotlib inline
```

## 练习 1：绘制最简单的折线图

给定数据：

```python
x = [1, 2, 3, 4, 5]
y = [2, 4, 6, 8, 10]
```

要求：

- 使用 `plt.plot()` 绘制折线图。
- 添加标题 `"Simple Line Plot"`。
- 横坐标标签为 `"x"`。
- 纵坐标标签为 `"y"`。
- 使用 `plt.show()` 显示图像。

提示：

```python
plt.title(...)
plt.xlabel(...)
plt.ylabel(...)
```

## 练习 2：修改线条样式

使用练习 1 的数据。

要求：

- 线条颜色设置为红色。
- 线条样式设置为虚线。
- 每个数据点用圆点标记。

提示：

```python
plt.plot(x, y, color=..., linestyle=..., marker=...)
```

## 练习 3：绘制两条函数曲线

给定数据：

```python
x = np.array([1, 2, 3, 4, 5])
y1 = x
y2 = x ** 2
```

要求：

- 在同一张图中画出 `y1` 和 `y2`。
- 给第一条线设置标签 `"y = x"`。
- 给第二条线设置标签 `"y = x^2"`。
- 显示图例。
- 添加标题 `"Two Lines"`。

提示：

```python
plt.plot(x, y1, label="...")
plt.plot(x, y2, label="...")
plt.legend()
```

## 练习 4：绘制散点图

给定数据：

```python
hours = [1, 2, 3, 4, 5, 6, 7]
scores = [52, 55, 61, 66, 70, 75, 80]
```

要求：

- 使用 `plt.scatter()` 绘制散点图。
- 横坐标为学习时间 `Study Hours`。
- 纵坐标为考试分数 `Score`。
- 点的颜色设置为蓝色。
- 点的形状设置为 `"x"`。
- 添加标题 `"Study Hours vs Score"`。

思考：

- 学习时间和考试分数之间看起来是什么关系？
- 如果要画一条拟合直线，应该用 `plt.plot()` 还是 `plt.scatter()`？

## 练习 5：散点图加拟合直线

继续使用练习 4 的数据。

给定一条简单预测直线：

```python
predicted_scores = [50, 55, 60, 65, 70, 75, 80]
```

要求：

- 用红色 `"x"` 画出真实数据点。
- 用蓝色实线画出预测分数。
- 给真实数据设置标签 `"Real data"`。
- 给预测直线设置标签 `"Prediction"`。
- 显示图例。

提示：

```python
plt.scatter(hours, scores, ...)
plt.plot(hours, predicted_scores, ...)
plt.legend()
```

## 练习 6：绘制柱状图

给定数据：

```python
subjects = ["Math", "English", "Physics", "Chemistry"]
scores = [88, 76, 92, 85]
```

要求：

- 使用 `plt.bar()` 绘制柱状图。
- 横坐标是科目名称。
- 纵坐标是分数。
- 标题为 `"Subject Scores"`。
- 柱子颜色设置为 `"skyblue"`。

思考：

- 哪门课分数最高？
- 柱状图适合表示类别数据还是连续数据？

## 练习 7：绘制水平柱状图

使用练习 6 的数据。

要求：

- 使用 `plt.barh()` 绘制水平柱状图。
- 横坐标标签为 `"Score"`。
- 纵坐标标签为 `"Subject"`。
- 标题为 `"Subject Scores"`。

提示：

```python
plt.barh(subjects, scores)
```

## 练习 8：绘制直方图

给定数据：

```python
np.random.seed(0)
data = np.random.randn(1000)
```

要求：

- 使用 `plt.hist()` 绘制直方图。
- 设置 `bins=30`。
- 柱子颜色设置为 `"lightblue"`。
- 边框颜色设置为 `"black"`。
- 标题为 `"Data Distribution"`。

思考：

- 这组数据大部分集中在哪个范围附近？
- `bins` 变大或变小时，图像会有什么变化？

## 练习 9：添加网格和坐标轴范围

给定数据：

```python
x = np.linspace(0, 10, 100)
y = np.sin(x)
```

要求：

- 绘制 `sin(x)` 曲线。
- 添加网格线。
- 横坐标范围设置为 `0` 到 `10`。
- 纵坐标范围设置为 `-1.5` 到 `1.5`。
- 标题为 `"Sine Function"`。

提示：

```python
plt.grid(True)
plt.xlim(...)
plt.ylim(...)
```

## 练习 10：绘制两个子图

给定数据：

```python
x = np.linspace(0, 10, 100)
y1 = np.sin(x)
y2 = np.cos(x)
```

要求：

- 使用 `plt.subplot()` 创建 1 行 2 列的子图。
- 左边画 `sin(x)`。
- 右边画 `cos(x)`。
- 两个子图都要有标题。

提示：

```python
plt.subplot(1, 2, 1)
plt.plot(x, y1)

plt.subplot(1, 2, 2)
plt.plot(x, y2)
```

## 练习 11：使用 `plt.subplots()`

继续使用练习 10 的数据。

要求：

- 使用 `fig, axes = plt.subplots(1, 2, figsize=(10, 4))`。
- 在 `axes[0]` 中画 `sin(x)`。
- 在 `axes[1]` 中画 `cos(x)`。
- 分别设置标题。

提示：

```python
fig, axes = plt.subplots(1, 2, figsize=(10, 4))
axes[0].plot(...)
axes[0].set_title(...)
```

思考：

- `plt.subplot()` 和 `plt.subplots()` 哪个更适合画复杂图？

## 练习 12：绘制损失函数下降曲线

给定一组模拟的损失函数值：

```python
cost_history = [10, 7.5, 5.8, 4.6, 3.8, 3.2, 2.8, 2.5, 2.3, 2.2]
```

要求：

- 使用 `plt.plot()` 绘制损失函数变化。
- 横坐标标签为 `"Iteration"`。
- 纵坐标标签为 `"Cost"`。
- 标题为 `"Cost Function During Training"`。
- 添加网格。
- 数据点用圆点标记。

思考：

- 这条曲线是否说明模型在学习？
- 如果损失函数越来越大，可能说明什么？

## 练习 13：保存图像

使用练习 12 的损失函数曲线。

要求：

- 将图像保存为 `"cost_function.png"`。
- 设置 `dpi=300`。
- 注意把 `plt.savefig()` 放在 `plt.show()` 之前。

提示：

```python
plt.savefig("cost_function.png", dpi=300)
```

## 练习 14：中文标题显示

给定数据：

```python
x = [1, 2, 3, 4, 5]
y = [3, 5, 6, 8, 9]
```

要求：

- 画一张折线图。
- 标题设置为 `"学习时间和成绩"`。
- 横坐标设置为 `"学习时间"`。
- 纵坐标设置为 `"成绩"`。
- 如果中文乱码，尝试设置字体。

提示：

```python
plt.rcParams["font.sans-serif"] = ["SimHei"]
plt.rcParams["axes.unicode_minus"] = False
```

## 练习 15：综合练习，画一张机器学习风格图表

给定数据：

```python
x = np.array([1, 2, 3, 4, 5, 6])
y = np.array([1.5, 2.0, 2.8, 4.1, 4.8, 6.2])
prediction = np.array([1.2, 2.1, 3.0, 3.9, 4.8, 5.7])
```

要求：

- 用散点图画出真实数据。
- 用折线图画出预测结果。
- 真实数据使用红色 `"x"`。
- 预测线使用蓝色。
- 添加标题 `"Linear Regression Result"`。
- 添加横纵坐标标签。
- 添加图例。
- 添加网格。
- 保存为 `"linear_regression_result.png"`。

检查点：

- 图中是否同时有真实数据和预测线？
- 图例是否能看懂？
- 横纵坐标是否有含义？
- 图片是否成功保存？

## 练习 16：自己设计一张图

请自己设计一组数据，并画出一张图。

可以选择：

- 折线图：例如每天学习时间变化。
- 散点图：例如学习时间和成绩关系。
- 柱状图：例如不同科目的成绩。
- 直方图：例如一组考试分数的分布。

要求：

- 至少包含标题。
- 至少包含横纵坐标标签。
- 尽量添加图例或网格。
- 试着调整颜色、线型或点的形状。

## 自查清单

完成练习后，检查自己是否掌握了：

- 会使用 `plt.plot()` 画折线图。
- 会使用 `plt.scatter()` 画散点图。
- 会使用 `plt.bar()` 和 `plt.barh()` 画柱状图。
- 会使用 `plt.hist()` 画直方图。
- 会添加标题、坐标轴标签和图例。
- 会添加网格和设置坐标轴范围。
- 会使用 `plt.subplot()` 或 `plt.subplots()` 画子图。
- 会使用 `plt.savefig()` 保存图片。
- 遇到图像不显示或中文乱码时，知道如何排查。

