---
title: Scanpy数据结构：AnnData
date: 2023-08-06 22:45:51
tags:
---

AnnData是python中存储单细胞数据的一种格式

##### 1. AnnData数据结构：

> 主要包含四个slots：
> `X` contains the expression matrix.
> `obsm` contains the embeddings data.
> `obs` contains the cell metadata.
> `var` contains the gene metadata.

![img](https://upload-images.jianshu.io/upload_images/15771939-738878fe0ad17428.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)



```dart
adata
# AnnData object with n_obs × n_vars = 2638 × 1838
#     obs: 'n_genes', 'n_genes_by_counts', 'total_counts', 'total_counts_mt', 'pct_counts_mt', 'leiden', 'louvain', 'louvain_anno'
#     var: 'gene_ids', 'n_cells', 'mt', 'n_cells_by_counts', 'mean_counts', 'pct_dropout_by_counts', 'total_counts', 'highly_variable', 'means', 'dispersions', 'dispersions_norm', 'mean', 'std'
#     uns: 'hvg', 'leiden', 'leiden_colors', 'neighbors', 'pca', 'rank_genes_groups', 'umap', 'draw_graph', 'diffmap_evals', 'louvain', 'paga', 'louvain_sizes', 'louvain_colors', 'leiden_sizes'
#     obsm: 'X_pca', 'X_umap', 'X_draw_graph_fa', 'X_diffmap'
#     varm: 'PCs'
#     obsp: 'connectivities', 'distances'

# 查看帮助文档和数据类型
help(adata)
type(adata.var["gene_ids"])
# pandas.core.series.Series
```

- `.X`这个部分储存的是矩阵信息，数据结构是`numpy array`，和seurat对象一样，基因variable *细胞observation的稀疏矩阵。
  但是.X的结构ndarray，是一个数组，是没有行名列名消息的。行名和列名消息存储在.obs和.var里。

调取矩阵信息：



```css
import scvelo as scv
scv.DataFrame(adata.X)
```

![img](https://upload-images.jianshu.io/upload_images/15771939-162cb9f01263eff9.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

- `.obs`存储的是细胞的信息，数据结构是`pandas dataframe`。
  相当于Seurat对象中的metadata



```css
adata.obs
```

![img](https://upload-images.jianshu.io/upload_images/15771939-08df5d5f7222e975.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

一维针对观测的注释，是数据框类型

访问其中的一个属性，就通过普通的dataframe来访问：



```css
adata.obs['n_genes']
AAACATACAACCAC-1     781
AAACATTGAGCTAC-1    1352
AAACATTGATCAGC-1    1131
AAACCGTGCTTCCG-1     960
AAACCGTGTATGCG-1     522
                    ... 
TTTCGAACTCTCAT-1    1155
TTTCTACTGAGGCA-1    1227
TTTCTACTTCCTCG-1     622
TTTGCATGAGAGGC-1     454
TTTGCATGCCTCAC-1     724
Name: n_genes, Length: 2638, dtype: int64
```

查看有多少cluster/细胞亚群...



```css
adata.obs['clusters'].unique()
```

- `.var`存储的是基因的信息，数据结构是`pandas dataframe`。



```csharp
adata.var
```

![img](https://upload-images.jianshu.io/upload_images/15771939-c35a34916e158070.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

一维的针对特征的注释，返回数据框类型。

访问其中一个属性的方法和.obs一样

- `.uns`存储的是后续添加的非结构信息，数据结构是`dict`，有序字典。



```kotlin
adata.uns['leiden']
# {'params': {'resolution': 1, 'random_state': 0, 'n_iterations': -1}}
adata.uns['leiden_colors']
# array(['#1f77b4', '#ff7f0e', '#279e68', '#d62728', '#aa40fc', '#8c564b', '#e377c2', '#b5bd61'], dtype=object)
```

这个uns的部分不是针对行/列的，而是针对行和列标注的参数的（暂时这么理解），在上述中pbmc的obs中有louvain观测，那么在uns中就是运行louvain算法的参数。是以哈希形式存储的。

- `obsm`
  对观测的**多维**的注释，m指的就是multi-dim多个维度。它是2-多维的ndarray，长度为n_obs。（obs是一个维度可以都放在一个数据框下）



```bash
adata.obsm
# AxisArrays with keys: X_pca, X_umap, X_draw_graph_fa
```



```bash
print(adata.obsm['X_pca'].shape,adata.obsm['X_umap'].shape)
adata.obsm['X_pca']
# (2638, 50) (2638, 2)
# array([[-5.5562196 , -0.257729  ,  0.18678935, ..., -0.33962035,
#          1.482267  ,  1.8977386 ],
#        [-7.209527  , -7.4819927 , -0.1626957 , ..., -1.9776347 ,
#         -1.5584233 , -1.4961302 ],
#        [-2.694437  ,  1.5836601 ,  0.6631187 , ...,  0.543645  ,
#         -0.54527736, -4.3395023 ],
#        ...,
#        [-0.78539336, -6.718588  , -1.5988318 , ..., -0.5611978 ,
#         -0.10546814,  0.58385324],
#        [ 0.2812712 , -5.921852  , -1.1628692 , ..., -1.3820586 ,
#          3.5802112 ,  1.2988565 ],
#        [-0.09076688, -0.66350466, -0.13485757, ...,  0.37319255,
#          0.75012326, -0.6659836 ]], dtype=float32)
```

- `varm`
  对特征的多维的注释，与obsm相对。



```bash
adata.varm
# AxisArrays with keys: PCs
print(adata.varm['PCs'].shape)
adata.varm['PCs']
# (1838, 50)
# array([[-2.6014808565e-02,  3.2541397959e-03,  1.8978352891e-03, ...,
#         -5.1610143855e-03,  1.4520395547e-02, -6.6632591188e-04],
#       [-8.2782376558e-03,  9.0831620619e-03, -7.8140682308e-04, ...,
#         3.0852310359e-02, -8.9336717501e-03, -2.8796317056e-03],
#       [-3.3151865937e-03,  3.2096833456e-03,  2.7985233464e-04, ...,
#         1.0144758970e-02, -5.5128755048e-04,  1.5089971712e-03],
#       ...,
#       [ 8.3417436108e-03, -1.2465091422e-03, -4.1219405830e-03, ...,
#        -1.0164264590e-02,  9.2523321509e-03,  2.7965774760e-02],
#       [-1.6406573355e-02,  4.4101417065e-02, -2.1357089281e-05, ...,
#         9.9819628522e-03, -4.5361258090e-03, -1.3653293252e-02],
#       [-1.5188260004e-02,  4.0008693933e-02,  5.4121930152e-03, ...,
#        -3.6789905280e-03,  2.1117802709e-02,  3.5965379328e-02]])
```

- `obsp`
  针对观测的配对的注释，前两维都是n_obs。比如点与点之间的距离和连通性。



```tsx
adata.obsp
# PairwiseArrays with keys: connectivities, distances
adata.obsp['connectivities']
# <2638x2638 sparse matrix of type '<class 'numpy.float32'>'
#   with 41952 stored elements in Compressed Sparse Row format>
adata.obsp['distances']
# <2638x2638 sparse matrix of type '<class 'numpy.float64'>'
#   with 23742 stored elements in Compressed Sparse Row format>
```

- `layers`
  在做速率分析的时候，还可以看到adata中有layers这一部分的信息。



```csharp
adata = scv.datasets.pancreas()
adata
AnnData object with n_obs × n_vars = 3696 × 27998
    obs: 'clusters_coarse', 'clusters', 'S_score', 'G2M_score'
    var: 'highly_variable_genes'
    uns: 'clusters_coarse_colors', 'clusters_colors', 'day_colors', 'neighbors', 'pca'
    obsm: 'X_pca', 'X_umap'
    layers: 'spliced', 'unspliced'
    obsp: 'distances', 'connectivities'
```

Dictionary-like object with values of the same dimensions as `X`.
可以理解为平行空间的对象，比如Seurat里面的count，标准化之后叫data，归一化之后叫scale.data。

##### 2. 提取信息

可以使用adata.然后输入Tab来查看可以使用的函数或变量。



```csharp
# 数据数目统计
adata.n_obs  # 返回细胞数 2695
adata.n_vars  # 返回基因数 18270
adata.shape # (2695, 18270)

# 数据键值提取
adata.obs_keys() # 细胞注释信息的keys，比如 ['ClusterID', 'ClusterName', 'SCT_snn_res_0_8', 'nCount_SCT', 'nCount_Spatial', 'nFeature_SCT', 'nFeature_Spatial', 'orig_ident', 'seurat_clusters', 'imagecol', 'imagerow'']
adata.obs_names  # 返回细胞ID 数据类型是object
adata.var.index  # 返回基因 数据类型是object
adata.var_names.to_list()  # 返回基因 数据类型是list
adata.obs.head() # 查看前5行的数据

# 其他的数据组成也可以使用
```

数据的索引和取切片等操作参考[numpy](https://www.jianshu.com/p/f2356f59b6e6)和[pandas](https://www.jianshu.com/p/926d819406ca)。