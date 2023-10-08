---
title: Dictys教程使用心得
banner:
  type: video
  bgurl: https://cdn.jsdelivr.net/gh/YesseniaCQ/img/video/心壑/心壑.m3u8
  bannerText: I'm stay with you.
date: 2023-08-25 00:34:02
tags:
cover:
---

- 使用 dictys_helper network_inference.sh，会自动对每个细胞子集进行分析（注意线程数）：
  - 对 RNA 数据进行质量控制和筛选，生成 expression.tsv.gz 文件。
  - 对 ATAC 数据进行质量控制和筛选，生成 names_atac.txt 文件。
  - 使用 macs2 程序对 ATAC 数据进行峰值检测，生成 reads.bam, reads.bai, peaks.bed 文件。
  - 使用 wellington 程序对 ATAC 数据进行足迹检测，生成 footprints.bed 文件。
  - 使用 dictys 模块对 RNA 和 ATAC 数据进行联合分析，生成 network.tsv.gz, network.h5, network.png, network.pdf, network.dot, network.gml, network.sif, network.tsv, network.html, network.json, network.cytoscape.json, network.graphml 文件。这些文件包含了 GRN 的结构和属性信息。

这张点图是用Dictys软件生成的，它是一个用于分析多组学数据的工具。它显示了不同细胞亚群之间的基因表达（以CPM为单位）和其特异性（以CPM的比例为单位）。每个点代表一个基因，点的大小和颜色反映了该基因在特定细胞亚群中的表达水平和特异性。这个图可以帮助发现每个细胞亚群的表达标记基因，这些基因在该细胞亚群中有较高的表达和特异性。这些标记基因可以反映细胞亚群的功能和特征，也可以用于进一步的分析和研究。

这张点图的主要信息有：

- **Treg细胞亚群**：这个细胞亚群主要表达了**FOXP3**，**CTLA4**，**IKZF1**等基因，这些基因与调节性T细胞（Treg）的功能有关。Treg细胞可以抑制其他免疫细胞的活动，从而维持免疫系统的平衡。
- **MP细胞亚群**：这个细胞亚群主要表达了**MAF**，**MAFB**，**EGR1**，**PPARA**等基因，这些基因与巨噬细胞（MP）的功能有关。巨噬细胞是一种能够吞噬和清除病原体和死亡细胞的免疫细胞。
- **NK.CD56h细胞亚群**：这个细胞亚群主要表达了**NFYC**，**ASCL2**，**HSF1**等基因，这些基因与自然杀伤细胞（NK）的功能有关。NK细胞是一种能够识别和杀死癌细胞和病毒感染的细胞的免疫细胞。
- **DC细胞亚群**：这个细胞亚群主要表达了**CUX2**，**ERG**，**MEIS1**，**PBX1**等基因，这些基因与树突状细胞（DC）的功能有关。DC细胞是一种能够捕获和呈递抗原给T细胞的免疫细胞。
- **Plasma细胞亚群**：这个细胞亚群没有明显的表达标记基因。
- **Th17细胞亚群**：这个细胞亚群主要表达了**SMAD3**，**NFIA**，**KLF9**等基因，这些基因与Th17型辅助T细胞（Th17）的功能有关。Th17细胞是一种能够分泌白介素17（IL-17）等炎症因子的免疫细胞。
- **Mono.CD14.1和Mono.CD14.2细胞亚群**：这两个细胞亚群都属于单核细胞（Mono），但是有不同的表达标记基因。Mono.CD14.1主要表达了**CEBPA**, **ETV7**, **CEBPG**, **NFIL3**, **BATF**, **CEBPB**, 等基因；Mono.CD14.2主要表达了 **E2F6**, **HES1**, **FOXJ2**, **DDIT3**, **MITF**, **IRF4**, 等基因。单核细胞是一种能够分化为巨噬细胞或树突状细胞的免疫细胞。
- **Tnaive细胞亚群**：这个细胞亚群没有明显的表达标记基因。
- **Tmem.CD4细胞亚群**：这个细胞亚群没有明显的表达标记基因。
- **NK.CD56l细胞亚群**：这个细胞亚群主要表达了**GFI1**，**BCL6**等基因，这些基因与自然杀伤细胞（NK）的功能有关。NK细胞是一种能够识别和杀死癌细胞和病毒感染的细胞的免疫细胞。
- **Tmem.CD8细胞亚群**：这个细胞亚群主要表达了**HINFP**，**TFAP4**，**MTF1**，**ETV3**，**SP1**，**MYC**，**TGIF1**等基因，这些基因与记忆性CD8+ T细胞（Tmem.CD8）的功能有关。记忆性CD8+ T细胞是一种能够在再次遇到同样的抗原时快速激活和扩增的免疫细胞。
- **B细胞亚群**：这个细胞亚群主要表达了**OVOL2**, **ZFP82**, **BRCA1**, **HSF2**, **SOX5**, **PLAG1**, **MYNN**, **PRDM1**, **PAX5**, **TFDP1**, 等基因，这些基因与B淋巴细胞（B）的功能有关。B淋巴细胞是一种能够分泌抗体的免疫细胞。
- **Mono.CD16细胞亚群**：这个细胞亚群主要表达了**ATF4**, **FOSL2**, **ZBTB4**, **ZNF41**, **MAFG**, **NR2C1**, **NFYA**, **USF2**, **DBP**, 等基因，这些基因与单核细胞（Mono）的功能有关。单核细胞是一种能够分化为巨噬细胞或树突状细胞的免疫细胞。

如果您想了解更多关于Dictys软件和点图的信息，可以参考以下链接：

- Dictys官网
- 点图介绍

请注意，这些链接可能包含英文内容。: https://dictys.org/ : https://www.nature.com/articles/nmeth.1615