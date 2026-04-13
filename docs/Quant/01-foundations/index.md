---
title: '第 1 章：Python 数据分析基础'
description: '掌握 Python 数据分析的必备技能：NumPy、Pandas、Matplotlib，为量化交易打下坚实基础。'
order: 1
---

# 第 1 章：Python 数据分析基础

## 本章概述

本章将帮助你掌握量化交易开发所需的 Python 数据分析技能。

这些技能包括：

- **NumPy**：高效的数值计算
- **Pandas**：数据处理的利器
- **Matplotlib/Seaborn**：数据可视化

## 本章导航

| 小节                                          | 内容                         | 核心交付             |
| --------------------------------------------- | ---------------------------- | -------------------- |
| [1.1 Python 基础回顾](./1.1-python-basics.md) | 变量、数据结构、函数、控制流 | 巩固 Python 基础     |
| [1.2 NumPy 数值计算](./1.2-numpy.md)          | 向量、矩阵、广播、统计函数   | 掌握 NumPy 核心用法  |
| [1.3 Pandas 数据处理](./1.3-pandas.md)        | Series、DataFrame、时间序列  | 掌握 Pandas 数据处理 |
| [1.4 数据可视化](./1.4-visualization.md)      | Matplotlib、Seaborn 基础     | 能够绘制金融图表     |

## 学习目标

完成本章学习后，你将能够：

1. ✅ 使用 NumPy 进行高效的数值计算
2. ✅ 使用 Pandas 处理金融时间序列数据
3. ✅ 使用 Matplotlib/Seaborn 绘制金融图表
4. ✅ 理解向量化编程的优势

## 重点概念

### 向量化 (Vectorization)

向量化是量化交易中最重要的编程思想之一：

```python
# 非向量化（慢）
result = []
for i in range(len(a)):
    result.append(a[i] + b[i])

# 向量化（快）
result = a + b
```

NumPy 和 Pandas 都为向量化操作进行了优化，在处理大规模金融数据时，**向量化代码比循环快 10-100 倍**。

### 时间序列索引

金融数据是典型的时间序列数据。Pandas 提供了强大的时间序列处理能力：

```python
# 按日期索引
df['2023-01':'2023-06']  # 选取特定日期范围
df.resample('M').mean()   # 按月重采样
df.rolling(20).mean()     # 20日滚动窗口
```

## 实践项目

完成本章学习后，你将完成一个简单的项目：

**项目：苹果股票数据分析**

- 获取 AAPL 一年历史数据
- 计算日收益率
- 绘制价格走势图和收益率分布图
- 计算关键统计指标

---

[进入 1.1：Python 基础回顾 →](./1.1-python-basics.md)
