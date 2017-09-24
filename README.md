# Additional File 2

## Introduction
This github repository includes R software codes used for a study on the evaluation of 14 differential gene expression methods for RNA-sequencing data (particularly for mRNA and lncRNA). Fourteen popular DGE tools are considered in our study. All considered tools are popular (in terms of number of citation) and available on R software packages. All of them start from gene or transcript level read counts. Most methods were applied using their default settings, except for DESeq2 and PoissonSeq which required certain settings to be changed to avoid filtering out too many low abundant genes (such as lncRNA). In particular, in DESeq2 the independent filtering was disabled in the results() function, and in PoissonSeq the filter cutoffs for the sum and average counts of genes across samples were set to 1 and 0.01, respectively. All DE tools were applied at the nominal 5% FDR level, unless mentioned otherwise. All computations were performed in R version 3.3.2. baySeq was not included in the simulation study due to its slow computation time.

The study has three main modules: (1) comparing selected normalization methods, (2) concordance analysis of 14 DGE analysis tools using 6 publicly accessible real RNA-seq data, and (3) non-parametric simulation study to evaluate 13 tools with respect to false discovery rate and sensitivity.

## Comparison of normalization methods
Five normalization methods that are used (as default) in conjunction to DGE tools (targeted tools in our study) are compared using two qualitative and one quantitative metrics: their capability to reduce variability that attributes to technical sources, their capability of eliminating bias due to library size differences, and their effect on DE analysis with the moderated t-test of the limma package. The normalization methods are  quantile normalization (QN) implemented in limma (limmaQN), Trimmed Mean of M-values  implemented in edgeR, limma (limmaVoom and limmaVoom+QW), baySeq, and QuasiSeq, Medians of Ratios (MR) implemented in DESeq, DESeq2, and limma (limmaVst), the goodness-of-fit statistics approach in PoissonSeq, and the re-sampling technique implemented in SAMSeq. Codes used for this module are compiled in a folder "comparing-normalization-methods".

## Concordance analysis of DE tools
Fourteen DE tools were run on 6 RNA-seq datasets, and similarities and dissimilarities among DE analysis results were examined. The concordance analysis focused on four quantitative and one qualitative metric: (1) number of genes identified as significantly differentially expressed (SDE); (2) the degree of agreement on gene ranking; (3) similarity of fold-change estimates; (4) treatment of genes with special characteristics (lncRNAs, genes with low counts, genes with outliers), and (5) computation time. The number of SDE genes detected for each dataset are compared among 14 DE tools. For the Zhang and NGP Nutlin datasets, we explored the proportion of mRNAs and lncRNAs within the set of SDE genes. For the other datasets, we explored the proportion of low and high expressed mRNAs among the set of SDE mRNAs. In particular, we divided the genes into 4 equally large groups based on the quartiles of their average normalized read counts across samples (DESeq normalization). To study the concordance in SDE calling between any two DE tools, we calculated the extent of overlap as the proportion of SDE genes commonly detected by the two tools. To rank genes in the order of significance of differential expression, taking into account the biological importance of the significance we calculated the $\pi$-score,
$$\pi_i= -\log_{10}p_i\times\phi_i$$ where $\phi_i$ is the absolute value of the estimated LFC for the $i^{th}$ gene and $p_i$ is the raw P-value, except for SAMSeq, for which $p_i$ is replaced by $w^{-1}_i$, where $w_i$ is the Wilcoxon statistic. For baySeq, $p_i$ is the estimated posterior probability of differential expression (estimated Bayesian False Discovery Rate, BFDR). Afterwards, we used Spearman's rank correlation to evaluate the degree of agreement among DE tools with respect to gene ranking. R codes used for this module are compiled in a folder named as "concordance-analysis"

## Non-parametric simulation
The non-parametric SimSeq (v 1.4.0) procedure was applied to realistically simulate RNA-seq expression data. The simulation technique involves sub-sampling of replicates from a real RNA-seq dataset with a sufficiently large number of replicates, which is referred to as the source dataset. In this way the underlying characteristics of the source dataset, including the count distributions and between genes associations, are preserved. Three series of simulations were performed, each starting from a different source RNA-seq dataset: Zhang, NGP Nutlin, and GTEx data. The degree of homogeneity among the replicates in these source datasets varies, suggesting that they have different levels of intra-group biological variability. The Zhang and NGP Nutlin datasets include annotated lncRNAs along with mRNAs, whereas the GTEx RNA-seq dataset contains only annotated mRNA genes.

Gene expressions were simulated under a wide range of scenarios that are believed to affect the performance of DE tools: different numbers of replicates ranging from 2 to 40, different proportions of true DE genes (0 to 30\%), two gene biotypes (mRNA and lncRNA), and different levels of intra-group biological variability (as present in the three source datasets). From the simulation results, the actual false discovery rate (FDR), and true positive rate (TPR), and false positive rate (FPR), were computed for all 13 DE tools. The comparison between the two gene biotypes was accomplished in two ways: first, we analyzed lncRNA and mRNA simulated counts jointly, and present the results for lncRNA and mRNA separately. Second, we simulated and analyzed only lncRNA expression data (thus removing mRNA before analysis).
