---
title: 生信R可视化
banner:
  type: video
  bgurl: https://cdn.jsdelivr.net/gh/YesseniaCQ/img/video/心壑/心壑.m3u8
  bannerText: I'm stay with you.
date: 2023-12-28 09:39:23
tags:
cover:
---
## 导出图片
```r
pdf("xx.pdf",width=5,height=8)
p1
dev.off()

ggsave("xx.xxx",width=5,height=8)
```

## 不同细胞类型差异基因表达的通路图
```r
library(Seurat)
library(magrittr)
library(ggplot2)
library(dplyr)
library(tidyr)
library(tidyverse)

df <- as.data.frame(AverageExpression(object = scrnat)$RNA)

df %>%
    filter(row.names(.) %in% all.markers$gene) %>%
    rownames_to_column(var = "gene") %>%
    right_join(all.markers) %>%
    arrange(cluster, avg_log2FC) %>%
    pivot_longer(cols = `0`:`16`, names_to = "cluster_name", values_to = "exp") %>%
    group_by(gene) %>%
    mutate(exp = as.numeric(scale(exp))) %>%
    ungroup() -> df2

p1 <- df2 %>%
    ggplot(aes(x = cluster_name,
               y = reorder(gene, cluster),
               fill = exp)) +
    geom_tile(color = "grey60") +
    scale_fill_gradient2(low = "blue", mid = "white", high = "red", breaks = c(-1,0,1,2)) +
    scale_x_discrete(limits = cluster_name) +
    theme_void() +
    theme(legend.position = "top")

```

```

  ![[Pasted image 20231229205030.png]]
这段R语言代码的主要目的是对一个数据框（data frame）`df` 进行一系列数据处理和转换操作。让我们一步一步来解释这些操作的含义：

1. `filter(row.names(.) %in% markers$gene)`：
    
    - `filter`函数用于筛选数据框中的行，这里是根据行的名称进行筛选。
    - `row.names(.)`返回数据框 `df` 的所有行的名称。
    - `markers$gene`是另一个数据框 `markers` 中的一列，它包含了基因名称。
    - 该行代码的作用是保留 `df` 中行名称在 `markers$gene` 中出现的行。
2. `rownames_to_column(var = "gene")`：
    
    - `rownames_to_column`函数用于将数据框的行名称添加为一个名为 "gene" 的新列。
3. `right_join(markers)`：
    
    - `right_join`函数用于执行右连接，将 `markers` 数据框的内容与 `df` 连接起来。
    - 连接的依据是它们共有的列，通常是 "gene" 列。
4. `arrange(cluster, avg_log2FC)`：
    
    - `arrange`函数用于对数据框的行进行排序。
    - 这里按照两列进行排序：首先按照 "cluster" 列排序，然后在同一 "cluster" 内按照 "avg_log2FC" 列排序。
5. `pivot_longer(cols = 'Naive CD4 T':'Platelet', names_to = "cluster_name", values_to = "exp")`：
    
    - `pivot_longer`函数用于将数据框从宽格式转换为长格式。
    - 这里将从列 'Naive CD4 T' 到 'Platelet' 的列转换为两列，其中 "cluster_name" 列包含了原来的列名，"exp" 列包含了相应的数值。
6. `group_by(gene)`：
    
    - `group_by`函数用于按照 "gene" 列的值对数据进行分组。
7. `mutate(exp = as.numeric(scale(exp)))`：
    
    - `mutate`函数用于在数据框中添加或修改列。
    - 这里将 "exp" 列的值进行标准化（scale），并将结果保存回 "exp" 列。
8. `ungroup()`：
    
    - `ungroup`函数用于取消之前使用 `group_by` 函数进行的分组操作，使数据框恢复到未分组状态。
9. `-> df`：
    
    - 将处理后的数据框保存回 `df`，覆盖原始的 `df`。

综合来看，这段代码的作用是对数据框 `df` 进行一系列数据操作，包括筛选、连接、排序、格式转换、标准化等，最终将处理后的结果保存回 `df` 变量中。这些操作通常用于数据清理和准备，以便后续的分析和可视化。

这段R语言代码的主要目的是创建一个基于数据框 `df` 的图形，通常是用于可视化数据的操作。让我们一步一步来解释这些操作的含义：

1. `filter(cluster_name != "NK", cluster != 6)`：
    
    - `filter`函数用于筛选数据框中的行。
    - `cluster_name != "NK"`筛选出 `cluster_name` 列不等于 "NK" 的行。
    - `cluster != 6`筛选出 `cluster` 列不等于 6 的行。
    - 这两个条件组合起来，用于过滤掉 `cluster_name` 列为 "NK" 且 `cluster` 列为 6 的行。
2. `ggplot(aes(x = cluster_name, y = reorder(gene, -cluster), fill = exp))`：
    
    - `ggplot`函数用于创建一个基础的绘图对象。
    - `aes`函数用于定义绘图的映射关系，包括 x 轴、y 轴和填充颜色。
    - `x = cluster_name` 将 `cluster_name` 列映射到 x 轴。
    - `y = reorder(gene, -cluster)` 将 `gene` 列映射到 y 轴，并根据 `cluster` 列的值进行重新排序。
    - `fill = exp` 将 `exp` 列的值映射到填充颜色。
3. `geom_tile()`：
    
    - `geom_tile`函数用于在绘图中创建矩形或瓷砖图。
    - 在这里，它用于创建矩形，其中每个矩形的位置由 `cluster_name` 和 `gene` 决定，矩形的颜色由 `exp` 决定。
4. `scale_fill_gradient2(low = "blue", mid = "white", high = "red")`：
    
    - `scale_fill_gradient2`函数用于设置填充颜色的渐变映射。
    - `low = "blue"` 设置低值颜色为蓝色。
    - `mid = "white"` 设置中间值颜色为白色。
    - `high = "red"` 设置高值颜色为红色。
5. `scale_x_discrete(limits = cluster_name)`：
    
    - `scale_x_discrete`函数用于设置 x 轴的刻度标签。
    - `limits = cluster_name` 设置 x 轴的刻度标签为 `cluster_name` 列中的唯一值，确保每个不同的 `cluster_name` 都有相应的刻度标签。

综合来看，这段代码的作用是创建一个基于数据框 `df` 的热图（heatmap）或瓷砖图，其中 x 轴表示 `cluster_name`，y 轴表示 `gene`（经过重新排序），并使用颜色来表示 `exp` 值的大小。通过过滤和美化操作，图形有助于可视化数据的模式和关系，以便更好地理解数据。