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

## 热图pheatmap
```r
pheatmap(
  mat = test, # 热图的数据源，要保证为数值矩阵，类型为numeric
  scale = "none", # 数值标准化，可以规定按行（row）或按列（column）
  clustering_method=""
  cluster_rows = TRUE, #  是否按行聚类
  cluster_cols = TRUE, #  是否按列聚类
  legend = TRUE, #  图例是否显示
  legend_breaks = NA, # 图例是否断点标注
  legend_labels = NA, # 图例断点标注的标题
  show_rownames = TRUE, # 是否显示行名
  show_colnames = TRUE, # 是否显示列名
  main = NA, #  热图标题
  fontsize = 10, #  热图字体大小
  fontsize_row = 10, #  热图行名字体大小，默认为fontsize
  fontsize_col = 10, #  热图列名字体大小，默认为fontsize
  angle_col = 0, #  热图列名角度
  display_numbers = FALSE, #  矩阵的数值是否显示在热图上
  number_format = "%.2f", # 单元格中数值的显示方式
  fontsize_number = 8, #  显示在热图上的矩阵数值的大小，默认为0.8*fontsize
  gaps_row = NULL, #  在行未聚类时使用，确定将间隙放置在热图中的位置。
  gaps_col = NULL, #  在列未聚类时使用，确定将间隙放置在热图中的位置。
  labels_row = NULL, #  使用行标签代替行名
  labels_col = NULL, #  使用列标签代替列名
  filename = NA, #  保存路径和文件名
  width = NA, # 保存图片的宽度
  height = NA, #  保存图片的高度
  na_col = "#DDDDDD" #  对“NA”值对应的单元格填充颜色
)
```
> 关于聚类的距离选择：
> 1. **correlation**：Pearson相关系数距离。适用于连续型数据，考虑基因之间的线性相关性。
> 2. **euclidean**：欧几里得距离。适用于连续型数据，将基因按照绝对表达值的差异进行排列。
>3. **maximum**：切比雪夫距离（Maximum Distance）。将基因按照最大差异值进行排列。
>4. **manhattan**：曼哈顿距离。适用于非负连续型数据，考虑基因之间的绝对差异,如基因表达数据，尤其在数据具有稀疏性（有大量零值）时。将基因按照它们的差异性进行排列。
>5. **canberra**：Canberra距离。适用于非负连续型数据，对差异的敏感性较高，可用于稀疏数据。
>6. **binary**：二进制距离。适用于二元数据，例如基因存在/不存在的情况。
>7. **minkowski**：闵可夫斯基距离。可以根据参数`p`来调整成其他距离度量，例如欧几里得距离（`p=2`）或曼哈顿距离（`p=1`）。

选择哪种距离度量取决于您的数据特点和分析目标。例如，如果您的数据是基因表达数据，通常使用`correlation`或`euclidean`距离进行聚类是常见的选择。如果您的数据具有不同的特性或需求，可以根据需要选择适当的距离度量。同时，也可以尝试不同的距离度量来查看它们如何影响热图的可视化结果，并选择最适合您研究的距离度量。
### 差异基因热图+GO富集
```r
#######################################################
library(ggplot2)
library(Seurat)
library(magrittr)
library(pheatmap)
#######################################################
  
## 单细胞表达文件
rna_file="/public/download/ycq2022/20231222_singleMuti/shareData/qc_atac/testis_combined.annotationCellType.qc.Rdata"

## 输出文件夹
out_path <- "/public/download/ycq2022/20240108_singleMuti/celltype_plot"

######################################################
load(rna_file)
DefaultAssay(scrnat) <- "MAGIC_RNA"

Idents(scrnat)<-"cell_type"
levels(x = scrnat) <- grp_order

######################################################

grp_order = c("SSC",
"Differenting&Differented SPG",
"Leptotene",
"Zygotene",
"Patchytene",
"Diplotene",
"Early stage of spermatids",
"Round&ElongateS.tids",
"Sperm",
"Leydig cells",
"Myoid cells",
"Pericytes",
"Sertoli cells",
"Endothelial cells",
"NKT cells",
"Macrophages")

#########################################################

# 寻找差异基因
markers <- FindAllMarkers(scrnat, test.use = "wilcox",
						  group.by = "cell_type",
						  min.pct = 0.25,
						  logfc.threshold = 0.25,
						  only.pos = TRUE,
						  verbose = TRUE)

markers.deseq2 <- FindAllMarkers(scrnat, test.use = "DESeq2",
                             group.by = "cell_type",
                             min.pct = 0.25,
                             logfc.threshold = 0.25,
                             only.pos = TRUE,
                             verbose = TRUE)
markers.fliter.fdr=markers[markers$p_val_adj<0.05,]
markers.fliter.fc=markers.fliter.fdr[markers.fliter.fdr$avg_log2FC>1,]
markers.fliter.fc[markers.fliter.fc["cluster"]=="Myoid cells"]
# 提取平均表达量
df <- as.data.frame(AverageExpression(object = scrnat)$MAGIC_RNA)

diff_gene <- markers.fliter.fc$gene
exp_diff <- df[rownames(df) %in% diff_gene,]
exp_diff <- exp_diff[diff_gene, grp_order]

#绘图
heatmap1=pheatmap(exp_diff, scale = "row", cluster_rows = F, cluster_cols = F, show_rownames = FALSE, color = colorRampPalette(colors = c("lightsteelblue1","white","red"))(100) , cutree_cols = 16)

heatmap2=pheatmap(exp_diff, scale = "row", clustering_method="single", cluster_rows = F, cluster_cols = F, show_rownames = FALSE, color = colorRampPalette(colors = c("lightsteelblue1","white","red"))(100) , cutree_cols = 16)

library(patchwork)

pdf("RNA_diffgene2.pdf")
heatmap2
dev.off()

```
### 气泡图
```r
library(Seurat)
library(magrittr)
library(ggplot2)
library(dplyr)
library(tidyr)
library(tidyverse)

markers = c('UTF1', 'KIT', 'STRA8', 'SPO11','SYCP3', 'OVOL2', 
                   'NME8', 'TXNDC8', 'TNP1',
                   'PRM1', 'AMH', 'DLK1', 
                   'MYH11',
                   'NOTCH3','CD14',
                   'NKG7', 'VWF', 
                   'FGFBP2')
library(stringr)  
p4 <- DotPlot(scrnat, features=markers,
             assay='RNA')  + coord_flip()

p4
ggsave(plot=p4, filename="check_marker_by_seurat_cluster.pdf")
```



```

[ggplot2参数简介_pythonggplot参数-CSDN博客](https://blog.csdn.net/weixin_44619692/article/details/131240952?utm_medium=distribute.pc_relevant.none-task-blog-2~default~baidujs_baidulandingword~default-0-131240952-blog-107055310.235^v40^pc_relevant_3m_sort_dl_base1&spm=1001.2101.3001.4242.1&utm_relevant_index=3)