
### QPCR Data Analysis



We analyzed the qPCR data. First, we calculated the Ct (Cycle Threshold) values. Ct values indicate the number of cycles required for the fluorescent signal to exceed the background level and are inversely proportional to the amount of target nucleic acid in the sample. Lower Ct values mean higher amounts of targer RNA. We obtained the following Ct values for different genes:

|          | ubi   | Rac1  | RhoA  | CDC42 | Rock1 | Vegf  | VegfR | RhoGap24l/2 |
|----------|-------|-------|-------|-------|-------|-------|-------|-------------|
| DMSO Control | 20.72 | 25.65 | 29.13 | 28.45 | 28.28 | 29.71 | 28.61 | 29.48       |
| Inhibitor Treatment | 19.89 | 25.34 | 28.41 | 27.38 | 28.01 | 28.85 | 28.36 | 30.45       |

Next, we calculated the ΔCt (Delta Ct) values, which represent the difference between the Ct value of the target gene and the Ct value of a reference gene (housekeeping gene). This step normalizes the data, accounting for variations in the amount of starting material and the efficiency of the PCR reaction. The ΔCt values are as follows:

|               | Rac1 | RhoA | CDC42 | Rock1 | Vegf | VegfR | RhoGap24l/2 |
|---------------|------|------|-------|-------|------|-------|-------------|
| DMSO Control  | 4.94 | 8.41 | 7.73  | 7.56  | 8.99 | 7.90  | 8.76        |
| Inhibitor Treatment | 5.44 | 8.51 | 7.48  | 8.11  | 8.95 | 8.47  | 10.56       |

Following that, we determined the ΔΔCt (Delta Delta Ct) values by comparing the ΔCt of the treatment sample to the ΔCt of the control sample. This allows for the calculation of relative gene expression levels. The ΔΔCt values are as follows:

| Genes | Rac1 | RhoA | CDC42 | Rock1 | Vegf | VegfR | RhoGap24l/2 |
|-------|------|------|-------|-------|------|-------|-------------|
| Ct    | 0.51 | 0.10 | -0.25 | 0.55  | -0.04| 0.57  | 1.79        |

Finally, we performed quantification of gene expression. Absolute quantification provides the exact number of copies of the target nucleic acid in the sample by comparing Ct values to a standard curve generated from known concentrations of the target. Relative quantification measures changes in gene expression levels relative to a control sample, typically using the ΔΔCt method. We calculated the fold change in gene expression using the 2^(-ΔΔCt) method, resulting in the following relative quantification (RQ) values:

| Genes | Rac1 | RhoA | CDC42 | Rock1 | Vegf | VegfR | RhoGap24l/2 |
|-------|------|------|-------|-------|------|-------|-------------|
| RQ    | 0.72 | 0.94 | 1.17 | 0.70  | 1.03 | 0.69  | 0.32        |



Only 2 genes that have the graph more than 1 can be quoted as the upregulated genes whetheras other are not considered as such. CDC42 and VEGF were upregulated rest were not.This is the analysis result.
