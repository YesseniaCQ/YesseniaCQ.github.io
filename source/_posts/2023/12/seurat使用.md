---
title: seurat使用
banner:
  type: video
  bgurl: https://cdn.jsdelivr.net/gh/YesseniaCQ/img/video/心壑/心壑.m3u8
  bannerText: I'm stay with you.
date: 2023-12-29 10:06:25
tags:
cover:
---
![[Pasted image 20240103183223.png]]
## 转录组处理
```r
###当前环境为outImg
load("../20231222_singleMuti/shareData/testis_combined.annotationCellType.Rdata")
table(Idents(scrnat)) #查看每种簇下的细胞数
Idents(scrnat)<-"cell_type" #修改active.ident:""内为seurat_obj@meta.data$cell_type


##对clusters进行重命名注释（注释来自文件）[没用上]
celltype <- read.csv("../celltype.csv",header=F)
labers = celltype[match(as.numeric(as.character(scrnat@active.ident)), celltype[, 1]), 2]
scrnat$labers <- labers
metadata <- scrnat@meta.data 
metadata$new_labers <- paste0('C', metadata$cluster, ': ', metadata$labers)
# Change cluster order 
a <- names(table(metadata$new_labers))
b <- gsub(":.*$", "", a)
c <- a[order(as.numeric(gsub("C", "", b)))]
metadata$new_labers2 <- factor(metadata$new_labers, levels=c)
scrnat@meta.data <- metadata
#画图
pdf("all_celltypes.pdf",width=6,height=5)
DimPlot(scrnat, reduction = "umap", group.by = "labers", label=T, label.size=5, pt.size=1) + ggtitle('Add celltype')
dev.off()

#marker基因提取
all.markers <- FindAllMarkers(scrnat, 
		                    only.pos = TRUE, 
                            min.pct = 0.25, 
                            logfc.threshold = 0.75)
top_genes <- head(rownames(seurat_object), 1000) # 选择前1000个差异表达基因
#celltype读入
celltype <- read.csv("",header=F)
cluster_name <- celltype[,2]
```

> FindAllMarkers和FindMarkers区别在于，前者all是比较所有clusters间的差异基因，后者是比较两个特定cluster间的差异基因
> - `logfc.threshold`：类群中基因的平均表达量相对于所有其他类群的平均表达量的最小log2倍数。默认值为0.25。
>	- 缺点：
>		- 如果平均log2fc不符合阈值，可能会错过那些在感兴趣的聚类中小部分细胞表达，但在其他聚类中没有表达的细胞标记。
>		- 由于不同细胞类型的代谢输出略有差异，可能会返回大量代谢/核糖体基因，这对区分细胞类型的身份没有那么有用。
>- `min.diff.pct`：该类群中表达该基因的细胞百分比与其他类群中表达该基因的细胞百分比之间的最小差异。
>	- 缺点：
>		- 可能漏掉那些在所有细胞中表达，但在该特定细胞类型中高度上调的细胞标记。
>- `min.pct`：只检验两个类群中任何一个类群中最少部分细胞中检测到的基因。旨在通过不检验那些表达频率很低的基因来加速函数运行。默认值为0.1。
>	- 缺点：
>		- 如果设置为一个很高的值，可能会产生很多假阴性，因为不是所有的基因都会在所有的细胞中检测到（即使是表达的基因）。

```r
count_mat <- GetAssayData(object=scrnat, slot="counts") #slot=counts/meta.data/data
avgPctMat <- AverageExpression(count_mat, scrnat$celltype, feature_normalize=TRUE, min_pct=0)

```

### 找差异基因
[seurat-FindAllMarkers()源码解析 - 简书 (jianshu.com)](https://www.jianshu.com/p/f5c8f9ea84af)
[单细胞转录组|Seurat 4.0 使用指南 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/359020041)
[[单细胞|R]单细胞之富集分析 - mdnice 墨滴](https://mdnice.com/writing/f645e2cad543426987f51b62c2ce2f9d)
```r
```
![[Pasted image 20240112232119.png]]