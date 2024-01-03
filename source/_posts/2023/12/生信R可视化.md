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

```r
library(ggplot2) 
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

Integrated single-cell chromatin and
transcriptomic analyses of human scalp
identify gene-regulatory programs and
critical cell types for hair and skin diseases`


```r
##########################################################################################
# Dot plots of both RNA and ATAC using common set of markers
##########################################################################################

# Dot plot of marker genes 

# NamedClust first:

# Markers for identifying broad classes of cells:
featureSets <- list(
    "Myoid cells" = c("MYH11"), 
    "Round&ElongateS.tids" = c("TXNDC8"), 
    "Sperm" = c("TNP1","PRM1"), 
    "Leydig cells" = c("DLK1"),  
    "Differenting&Differented SPG" = c("KIT","STRA8"), 
    "SSC" = c("UTF1"), 
    "Endothelial cells" = c("VWF"), 
    "Leptotene" = c("SPO11"), 
    "Zygotene" = c("SYCP3"), 
    "Diplotene" = c("NME8"), 
    "Sertoli cells" = c("AMH"),
    "Patchytene" = c("OVOL2"),
    "Macrophages" = c("CD14"),
    "Pericytes" = c("NOTCH3"),
    "NKT cells" = c("NKG7","GFBP2")
)

NatacOrder <- c(
    "0",
    "1",
    "2",
    "3",
    "4",
    "5",
    "6",
    "8",
    "9",
    "10",
    "11",
    "12",
    "13",
    "14",
    "15",
    "16"
)

NrnaOrder <- c(
    "rTc3", 
    "rTc2", 
    "rTc1", 
    "rBc1", 
    "rMy4", 
    "rMy2", 
    "rMy1", 
    "rMy3", 
    "rMa1", 
    "rKc5", 
    "rKc4", 
    "rKc1", 
    "rKc2", 
    "rKc3", 
    "rFb1", 
    "rFb2", 
    "rMu1", 
    "rMu2", 
    "rVe1", 
    "rLe1", 
    "rMe1", 
    "rMe2" 
)
LNatacOrder <- unlist(atac.NamedClust)[NatacOrder]
LNrnaOrder <- unlist(rna.NamedClust)[NrnaOrder]

namedClustAspect <- 1.6
fineClustAspect <- 1.6



```

[ggplot2参数简介_pythonggplot参数-CSDN博客](https://blog.csdn.net/weixin_44619692/article/details/131240952?utm_medium=distribute.pc_relevant.none-task-blog-2~default~baidujs_baidulandingword~default-0-131240952-blog-107055310.235^v40^pc_relevant_3m_sort_dl_base1&spm=1001.2101.3001.4242.1&utm_relevant_index=3)