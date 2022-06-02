# Phylosymbiosis and Coevolution

Phylosymbiotic and Cophylogenetic analyses for host-microbe interactions.

![](https://thesaurus.plus/img/synonyms/183/coevolution.png)

With these scripts you would be able to conduct Phylosimbiotic analysis between your host and the microbial community associated. Additionally, to test host-microbe coevolution.

Better explanaitions in our wiki: [Wiki](https://github.com/DianaCarolinaVergara/Phylosymbiosis_and_Coevolution/wiki)

First a little theorical introduction.

### Phylosymbiosis

Phylosymbiosis is a pattern in many host-microbial symbiosis studies with correlations between host phylogenetic relationships and microbial community composition. Phylosymbiosis has been reported in plants, insects, mammals and in marine organisms as sponges and hard corals. Phylosymbiotic patterns can be explained by several mechanisms, including codiversification of abundant microbial lineages with their hosts, filtering of microbial communities by host traits, or coupling between host phylogeography and environmental effects on the microbiome(1).

![](https://journals.plos.org/plosbiology/article/file?id=10.1371/journal.pbio.2000225.g001&type=large)

Fig 1. doi: https://doi.org/10.1371/journal.pbio.2000225.g001

> Host-associated microbiota composition can be conserved over evolutionary
time scales. Indeed, closely related species often host similar microbiota; i.e.,
the composition of their microbiota harbors a phylogenetic signal, a pattern sometimes
referred to as “phylosymbiosis.” (2)

Exists evidence of coral-microbe phylosymbiosis, in which coral microbiota reflect coral phylogeny. With our data we tests the same scenario between soft corals and its microbial community composition and richness.

For this Phylosymbiotic analysis we perform a **Mantel Test** in [Qiime1](http://qiime.org/), [Qiime2](https://qiime2.org/) or in [R](https://rstudio.com/).

You need the previous files obtained in the [16S rRNA Analysis](https://github.com/DianaCarolinaVergara/16S-rRNA-Analysis), especifically the files obtained in the [Beta diversity analysis](https://github.com/DianaCarolinaVergara/16S-rRNA-Analysis/wiki/8.2-Beta-Analysis), the Bray-Curtis `distance matrix`. Also need the `host phylogeny`, the result of SNPs Analysis and phylogeny inference (ML).

#### MANTEL Test

The evaluated phylosymbiotic eco-evolutionary pattern, between microbial community and host with the Mantel test to correlate the microbial β-diversity and host phylogeny. The [mantel-test](github.com/DianaCarolinaVergara/Phylosymbiosis_and_Coevolution/wiki/3.-Phylosymbiosis) comparing the degree of similarity between evolutionary distance matrices showed correlation (with or without Bonferroni correction).

```
qiime tools export  --input-path "/hpcfs/home/dc.vergara10/dc.vergara10/Muricea/SNPs/bray_curtis_distance_matrix.qza" --output-path "/hpcfs/home/dc.vergara10/dc.vergara10/Muricea/SNPs/"
```
or

```
qiime diversity mantel \
  --i-dm1 rooted-tree.qza
  --i-dm2 bray_curtis_distance_matrix.qza
  ```
  #### MANTEL Test PER GEOGRAPHIC LOCATION
  
  
 ##### Files:
 - microbes matrix (.txt converted to .qza)
 - Muricea host matrix (.txt converted to .qza)

#convert matrix .txt to .qza 

```
qiime tools import     --input-path /home/vergarad/Muricea/Matrices/CARIBE_distance-matrix_BACTERIA.txt     --output-path CARIBE_bacteria_distance_matrix.qza     --type DistanceMatrix
```
 
  * Caribe
qiime diversity mantel   --i-dm1 /home/vergarad/Muricea/CARIBE_host_distance_matrix_comas.qza --i-dm2 /home/vergarad/Muricea/CARIBE_bacteria_distance_matrix_comas.qza --output-dir /home/vergarad/Muricea/Caribe

  * TEP

```
qiime diversity mantel   --i-dm1 /home/vergarad/Muricea/TEP_host_distance_matrix_comas.qza --i-dm2 /home/vergarad/Muricea/TEP_bacteria_distance_matrix_comas.qza --output-dir /home/vergarad/Muricea/TEP

```

  * Gulf-Mexico
  
 ```
qiime diversity mantel   --i-dm1 /home/vergarad/Muricea/GULF-MEXICO_host_distance_matrix_comas.qza --i-dm2 /home/vergarad/Muricea/GULF-MEXICO_bacteria_distance_matrix_comas.qza --output-dir /home/vergarad/Muricea/Gulf_Mex
 ```
_____________
  
### Coevolution

Coevolution is used to describe cases where two (or more) species reciprocally affect each other's evolution. Coevolution is likely to happen when different species have close ecological interactions with one another. These ecological relationships include:

- **Host/Microbe**
- Predator/prey and parasite/host
- Competitive or Mutualistic species


![](https://www.researchgate.net/profile/Anurag_Agrawal5/publication/26881487/figure/fig1/AS:601658455228434@1520457962604/A-conceptualization-of-escape-and-radiate-coevolution-hypothesized-by-Ehrlich-and-Raven_W640.jpg)

Fig 2. Coevolution conceptualization. [DOI](https://www.researchgate.net/publication/26881487_Macroevolution_and_the_biological_diversity_of_plants_and_herbivores/figures?lo=1)

For the cophylogenetic analysis, we used a **Procrustean Superimposition of topologies** and a Goodness-of-fit test. With this methodology, the trees (host and bacterial tree) were transformed into the matrices of Principal Coordinates (PCoA); **PACo** was applied and a Procrustes fit was carried out. Then the residual sum of squares (m2XY) was computed, and randomization of the host-bacteria association matrix was performed to establish the probability with 100,000 randomizations for high precision of the P estimate. 

A superimposition plot as a visual indication of the degree of match between the two ordinations was generated. 

#### PACo Test

For this Cophylogenetic analysis we perform a **[PACo Test](https://github.com/DianaCarolinaVergara/Phylosymbiosis_and_Coevolution/wiki/4.-Cophylogeny)** in [R](https://rstudio.com/).

You need the previous files obtained in the [16S rRNA Analysis](https://github.com/DianaCarolinaVergara/16S-rRNA-Analysis), especifically the phylogentic `fasttree` in the [Construct a Phylogeny analysis](https://github.com/DianaCarolinaVergara/16S-rRNA-Analysis/wiki/10.-Construct-a-Phylogeny). Also need the `host phylogeny`, the result of SNPs Analysis and phylogeny inference (ML).

> in RStudio

```
library(ape)
library(vegan)
```

```
TreeH <- read.tree("HOST_tree.txt")
TreeP <- read.tree("BACTERIA_tree.txt")
HP <- as.matrix(read.table("Binary_PACo_coevolution.txt", header=TRUE))
NLinks = sum(HP)
host.D <- cophenetic(TreeH)
para.D <- cophenetic(TreeP)
```

```
PACo <- function (H.dist, P.dist, HP.bin)
{HP.bin <- which(HP.bin > 0, arr.in=TRUE)
H.PCo <- pcoa(H.dist, correction="cailliez")$vectors
P.PCo <- pcoa(P.dist, correction="cailliez")$vectors
H.PCo <- H.PCo[HP.bin[,1],]
P.PCo <- P.PCo[HP.bin[,2],]
list (H.PCo = H.PCo, P.PCo = P.PCo)}
```

And Procrustes-fit

```
PACo.fit <- PACo(host.D, para.D, HP)
HP.proc <- procrustes(PACo.fit$H.PCo, PACo.fit$P.PCo)
HP.proc
```

First, you need to check that you have:
- [X] Installed qiime2 and qiime1 at your HPC cluster
- [X] Installed RStudio in your desktop or HPC cluster environment.
- [X] Load a conda environment `module load anaconda/python3.7`
- [X] Activate qiime2 source `source activate qiime2-2019.7`
- [X] Load R packages: `ape`, `vegan`
- [ ] Preferable start an interactive session or a job.

.

![](https://qiime2.org/assets/img/q2cli.png)
![qiime2](https://qiime2.org/assets/img/qiime2.svg)
![](http://ape-package.ird.fr/image/new_logo_gold.png)

_____________

#### References

1. Coral-associated bacteria demonstrate phylosymbiosis and cophylogeny DOI: 10.1038/s41467-018-07275-x

2. Is Host Filtering the Main Driver of Phylosymbiosis across the Tree of Life? https://doi.org/10.1128/mSystems.00097-18.

3. https://evolution.berkeley.edu/evolibrary/article/evo_33

4. PACo: A Novel Procrustes Application to Cophylogenetic Analysis. https://journals.plos.org/plosone/article?id=10.1371/journal.pone.0061048
