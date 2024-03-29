---
title: GWAS全基因组关联研究相关整理
banner: https://cdn.jsdelivr.net/gh/YesseniaCQ/img/video/心壑/心壑.m3u8
date: 2023-10-12 16:57:02
tags:

---

> 参考文档：[进行全基因组关联研究的教程：质量控制和统计分析 - Marees - 2018 - 国际精神病学研究方法杂志 - Wiley 在线图书馆](https://onlinelibrary.wiley.com/doi/full/10.1002/mpr.1608)

SNP可以概括为：首先，将DNA样本提取出来，并将其分成两个部分：病例组(case)和对照组(contral)。然后，将这些DNA样本放在GWAS芯片上进行检测。最后，将检测结果与疾病或特定性状进行关联分析，以确定哪些基因位点与该疾病或特定性状有关

主要分为4部分。

**第一部分：测序**

- Sequencing就是指测序，<u>一般GWAS使用的都是基因**芯片**</u>（chip is cheap），芯片上排列着大量已经设计好的SNP 位点（SNP array），一般可以有上百万个。

- GWAS芯片的设计是基于标签变异的，即那些与疾病或特定性状相关的**常见变异**。而<u>特定人种</u>（因为同一SNP在不同人种中的频率可能相差很大）一般使用<u>特制的芯片</u>。比如，英国生物银行（UK Biobank）主要使用UK Biobank Axiom array这款自制芯片测了约45万人，而针对亚洲人一般使用Illumina Asian Screening Array 这款芯片（Illumina是基因芯片公司）。因此，如果想自己测序，一定要选好合适的芯片。测完序并经过配套软件处理后我们通常会得到原始的测序数据。
- 不过特制芯片成本太大，**罕见变异**可以直接进行测序（WGS、WES）

**第二部分：质控和SNP calling**

拿到原始测序数据后，质控（QC）通常按照GATK的推荐流程进行。做完SNP calling后我们可以得到vcf（Variant Call Format）格式文件。

七个QC步骤包括根据以下内容过滤出SNP和个体(`--geno`筛选SNP；`--mind`筛选样本个体)：

（1）**个体和SNP缺失**

（2）受试者指定和遗传性别的不一致（见**性别差异**）

（3）**次要等位基因频率（MAF）**

（4）与**Hardy-Weinberg均衡（HWE**）的偏差

（5）**杂合**率

（6）**相关性**

（7）种族异常值（见**群体分层**）

> 对SNP缺失超过阈值的**个体**，或在样本中分型缺失超过阈值的**SNP**，都要进行质控
>
> - `--geno`筛选SNP；`--mind`筛选样本个体
> - SNP过滤->SNP calling->个体过滤
>
> - SNP分型：确定每个SNP位点的基因型

**第三部分：Association analysis**

对于vcf文件，我们可以使用vcftools这个软件将其转化为PLINK格式的二进制文件，数据特别大的时候可以存储为BGEN格式（UK Biobank使用的就是BGEN格式）。这里我们默认大家拥有PLINK格式或者BEGN格式的数据了，在进行关联分析之前，我们可以使用qctools这个软件来对数据进行质控（PLINK个GCTA软件也可进行质控）。在完成质控后，我们就可以使用PLINK或者GCTA软件进行关联分析了，最后我们会得到单个SNP与表型的关联结果，也就是进行MR分析时需要的summary statistics。

如果SNP array得到的位点数太少，这时候我们是需要进行基因填充的（imputation），一般使用IMPUTE2这个软件，它可以依据参考基因组的信息推断出那些不在芯片上的位点在人群中的分布情况，这样原来只有100万个SNP位点的芯片数据经过填充后可能有超过1000万个位点信息。

1. 关联分析：将基因型数据与疾病或特定性状进行关联分析，以确定哪些基因位点与该疾病或特定性状有关。
2. 功能注释：对关联分析结果进行功能注释，以确定哪些基因位点可能对疾病或特定性状产生影响。

### 数据文件内容

PLINK 可以读取文本格式的文件或二进制文件。由于读取大型文本文件可能很耗时，因此建议使用二进制文件。文本PLINK数据由两个文件组成：一个包含有关个体及其基因型的信息（*.ped）;另一个包含有关遗传标记的信息（*.map;见图[1](https://onlinelibrary.wiley.com/doi/full/10.1002/mpr.1608#mpr1608-fig-0001)）。相比之下，二进制PLINK数据由三个文件组成，一个包含个体标识符（ID）和基因型（*.bed）的二进制文件，以及两个包含个体信息（*.fam）和遗传标记信息的文本文件（*.bim;见图[1](https://onlinelibrary.wiley.com/doi/full/10.1002/mpr.1608#mpr1608-fig-0001)）。例如，在一项双相情感障碍的研究中，*.bed 文件将包含所有患者和健康对照的基因分型结果;*.fam文件将包含与受试者相关的数据（与研究中其他参与者的家庭关系，性别和临床诊断）;而 *.bim 文件将包含有关 SNP 物理位置的信息。 使用协变量进行分析通常需要第四个文件，其中包含每个个体的这些协变量的值（见图 [1](https://onlinelibrary.wiley.com/doi/full/10.1002/mpr.1608#mpr1608-fig-0001)）

### 校正：

群体分层

样本来自不同人群的情况。由于不同人群之间的基因型差异，群体分层可能会导致虚假关联。为了解决这个问题，研究者通常会使用一些方法来校正群体分层，例如主成分分析（PCA）

亲缘分层

亲缘关系会导致基因型之间的相关性，因此在GWAS中需要考虑亲缘分层的影响。为了解决这个问题，研究者通常会使用一些方法来校正亲缘分层，例如基于家系的方法或基于单倍型的方法



___

如果一个变异与芯片已有变异有连锁不平衡（LD）关联，那么它可能会被GWAS芯片检测到。然而，如果罕见变异与芯片已有变异没有LD关联，那么它们可能不会被包含在GWAS芯片中