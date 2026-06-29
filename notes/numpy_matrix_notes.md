# NumPy 中 `matrix` 的入门笔记

这份笔记配合 `ML-Exercise1.ipynb` 看，重点理解线性回归里经常出现的矩阵、向量、转置、矩阵乘法和参数更新。

## 1. `np.matrix` 是什么

`np.matrix` 是 NumPy 里专门表示二维矩阵的类型。

```python
import numpy as np

A = np.matrix([[1, 2],
               [3, 4]])

print(A)
print(A.shape)
```

输出形状是：

```text
(2, 2)
```

它和普通的 `np.array` 最大的区别是：

- `np.matrix` 永远是二维的。
- 对 `np.matrix` 来说，`*` 表示矩阵乘法。
- 对普通 `np.array` 来说，`*` 表示逐元素相乘。

不过，现在 NumPy 官方更推荐使用 `np.array` 或 `ndarray`，再用 `@` 做矩阵乘法。你现在的课程代码里使用 `np.matrix`，主要是为了让线性代数公式看起来更接近数学写法。

## 2. 创建矩阵

### 从列表创建

```python
A = np.matrix([[1, 2, 3],
               [4, 5, 6]])
```

这是一个 2 行 3 列的矩阵：

```python
A.shape
```

```text
(2, 3)
```

### 从数组创建

课程代码里有类似写法：

```python
theta = np.matrix(np.array([0, 0]))
```

这会得到一个 1 行 2 列的矩阵：

```text
matrix([[0, 0]])
```

注意：`np.array([0, 0])` 原本是一维数组，形状是 `(2,)`。转换成 `np.matrix` 后，会变成二维矩阵，形状是 `(1, 2)`。

## 3. 行向量、列向量和形状

机器学习里非常容易因为形状不匹配报错，所以要经常看 `.shape`。

```python
row = np.matrix([[1, 2, 3]])
col = np.matrix([[1],
                 [2],
                 [3]])

print(row.shape)
print(col.shape)
```

结果：

```text
(1, 3)
(3, 1)
```

行向量：

```text
1 行，n 列
```

列向量：

```text
n 行，1 列
```

在线性回归中，常见约定是：

- `X`：训练样本矩阵，形状通常是 `(m, n)`。
- `y`：目标值，形状通常是 `(m, 1)`。
- `theta`：参数，可能是 `(1, n)` 或 `(n, 1)`，具体取决于代码写法。

这里：

- `m` 表示样本数量。
- `n` 表示特征数量，包括偏置项那一列 `1`。

## 4. 转置 `.T`

转置会把行和列交换。

```python
A = np.matrix([[1, 2, 3],
               [4, 5, 6]])

A.T
```

原来 `A.shape` 是：

```text
(2, 3)
```

转置后 `A.T.shape` 是：

```text
(3, 2)
```

在正规方程里，经常看到：

```python
theta = np.linalg.inv(X.T @ X) @ X.T @ y
```

对应数学公式：

```text
theta = (X^T X)^(-1) X^T y
```

其中：

- `X.T` 表示 `X` 的转置。
- `np.linalg.inv(...)` 表示求逆矩阵。
- `@` 表示矩阵乘法。

## 5. 矩阵乘法：`*`、`@` 和 `.dot()`

### `np.matrix` 中的 `*`

对 `np.matrix` 来说：

```python
A * B
```

表示矩阵乘法。

但是对普通 `np.array` 来说：

```python
A * B
```

表示逐元素相乘。

所以为了避免混淆，建议你养成习惯：矩阵乘法优先写 `@`。

### 推荐写法：`@`

```python
A @ B
```

这表示矩阵乘法，读起来也最接近数学里的矩阵相乘。

例如：

```python
X @ theta.T
```

可以理解为：

```text
用每个样本的特征，乘上参数 theta，得到预测值。
```

### `.dot()`

下面两种写法通常等价：

```python
X.T @ X
X.T.dot(X)
```

课程代码里注释也提到了：

```python
X.T @ X  # 等价于 X.T.dot(X)
```

新手阶段建议先统一使用 `@`，更直观。

## 6. 逐元素运算

机器学习里也经常需要逐元素运算，例如预测误差：

```python
error = X @ theta.T - y
```

如果 `X @ theta.T` 和 `y` 都是 `(m, 1)`，那么：

```python
error
```

也是 `(m, 1)`。

平方误差：

```python
error_squared = np.power(error, 2)
```

或者：

```python
error_squared = np.square(error)
```

这里不是矩阵乘法，而是每个误差值分别平方。

## 7. 在线性回归中的典型形状

假设有 `m` 个样本，1 个特征，再加上一列偏置项 `1`。

那么：

```text
X.shape      = (m, 2)
y.shape      = (m, 1)
theta.shape  = (1, 2)
```

预测值通常这样算：

```python
predictions = X @ theta.T
```

形状变化：

```text
X              (m, 2)
theta.T        (2, 1)
X @ theta.T    (m, 1)
```

这正好和 `y` 的形状一致，所以可以相减：

```python
error = predictions - y
```

## 8. 梯度下降中的矩阵理解

线性回归假设函数：

```text
h_theta(x) = theta_0 + theta_1 x
```

写成矩阵形式：

```python
predictions = X @ theta.T
```

误差：

```python
error = predictions - y
```

参数更新的核心想法是：

```text
theta_j := theta_j - learning_rate * gradient_j
```

如果用循环写，会看到类似：

```python
term = np.multiply(error, X[:, j])
theta[0, j] = theta[0, j] - (alpha / len(X)) * np.sum(term)
```

这里：

- `X[:, j]` 表示取出第 `j` 个特征列。
- `error` 是每个样本的预测误差。
- `np.multiply(error, X[:, j])` 表示逐元素相乘。
- `np.sum(term)` 把所有样本对这个参数的影响加起来。

重点区分：

- `@`：矩阵乘法。
- `np.multiply(...)`：逐元素相乘。

## 9. `np.matrix` 的几个常见坑

### 坑 1：`matrix` 永远是二维

```python
a = np.array([1, 2, 3])
b = np.matrix([1, 2, 3])

print(a.shape)
print(b.shape)
```

结果：

```text
(3,)
(1, 3)
```

`array` 可以是一维，`matrix` 会变成二维行矩阵。

### 坑 2：`*` 的含义容易混淆

```python
A = np.array([[1, 2],
              [3, 4]])

B = np.array([[10, 20],
              [30, 40]])

A * B
```

这是逐元素相乘。

但如果是：

```python
A = np.matrix(A)
B = np.matrix(B)

A * B
```

这就是矩阵乘法。

所以建议尽量写：

```python
A @ B
```

这样不管你用的是 `array` 还是 `matrix`，语义都更清楚。

### 坑 3：形状不匹配

矩阵乘法要求：

```text
(a, b) @ (b, c) -> (a, c)
```

中间两个维度必须一样。

比如：

```text
(97, 2) @ (2, 1) -> (97, 1)
```

可以相乘。

但：

```text
(97, 2) @ (1, 2)
```

不能相乘，因为中间维度 `2` 和 `1` 不一致。

遇到报错时，第一件事就是打印：

```python
print(X.shape)
print(y.shape)
print(theta.shape)
```

## 10. 现在更推荐的写法

虽然课程里用了：

```python
X = np.matrix(X.values)
y = np.matrix(y.values)
theta = np.matrix(np.array([0, 0]))
```

但现代 NumPy 更常见的写法是：

```python
X = X.values
y = y.values
theta = np.array([[0, 0]])
```

矩阵乘法写：

```python
predictions = X @ theta.T
```

也就是说：

- 旧课程或老代码里可能看到 `np.matrix`。
- 新代码里更推荐用 `np.array` 加 `@`。
- 学习阶段可以先理解 `matrix`，但以后写项目时尽量用 `array`。

## 11. 小抄

```python
import numpy as np

A.shape           # 查看形状
A.T               # 转置
A @ B             # 矩阵乘法，推荐
A.dot(B)          # 矩阵乘法的另一种写法
np.multiply(A, B) # 逐元素相乘
np.power(A, 2)    # 逐元素平方
np.sum(A)         # 所有元素求和
np.linalg.inv(A)  # 求逆矩阵
```

最重要的习惯：

```python
print(X.shape, y.shape, theta.shape)
```

机器学习初学阶段，很多问题不是公式错了，而是矩阵形状没有对齐。

