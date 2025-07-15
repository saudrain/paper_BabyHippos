# The paper
This repository includes scripts and derivative data for the following paper:

Audrain, S., Milleville, S., Wilson, J., Baffoe-Bonnie, J., Gotts, S., & Martin, A. (2024). The development of functional connectivity along the hippocampal long-axis in infants. ResearchSquare.

# Abstract
Infancy is a critical period for memory development, yet the functional neural changes that occur during this time remain poorly understood. In adults, long-term memory relies on hippocampal-neocortical coupling, which differs along the hippocampal long-axis. We examined resting-state connectivity in 212 infants and toddlers during the first two postnatal years. Hippocampal connectivity with canonical adult memory regions was largely established by 6 months, with early signs of functional differentiation along the hippocampal axis. However, we found distinct maturation trajectories across anterior and posterior hippocampal connections, particularly in medial temporal, medial parietal, and social, salience, and attention-related regions. Comparing toddlers and children aged 3-6 years revealed increased hippocampal connectivity with adult-like memory, attention, and salience networks in older children. These findings chart the developmental trajectory of hippocampal-cortical coupling and raise the possibility that extended functional interactions among memory, attention, and salience systems may contribute to the emerging ability to form long lasting memories.

# Software

Adjacency matrices needed for the clustering consistency analyses were extracted using Matlab 2021b.

* Matlab can be downloaded from here: https://www.mathworks.com/downloads

Statistical analyses and plots were computed using R Studio version 2023.12.1.402.

* R and R studio can be downloaded here: https://www.rstudio.com/products/rstudio/download/
* R packages required:
  - dplyr: https://cran.r-project.org/web/packages/dplyr/index.html
  - tidyr: https://cran.r-project.org/web/packages/tidyr/index.html
  - nlme: https://cran.r-project.org/web/packages/nlme/index.html
  - emmeans: https://cran.r-project.org/web/packages/emmeans/index.html
  - Rmisc: https://cran.r-project.org/web/packages/Rmisc/index.html
  - car: https://cran.r-project.org/web/packages/car/index.html
  - ggplot2: https://cran.r-project.org/web/packages/ggplot2/index.html
  - stringr: https://cran.r-project.org/web/packages/stringr/index.html
  - factoextra: https://cran.r-project.org/web/packages/factoextra/index.html
  - fpc: https://cran.r-project.org/web/packages/fpc/index.html
  - cluster: https://cran.r-project.org/web/packages/cluster/index.html
  - cowplot: https://cran.r-project.org/web/packages/cowplot/index.html
  - corrplot: https://cran.r-project.org/web/packages/corrplot/index.html

These software packages must be installed on your local machine in order to run these scripts. Installation can take several minutes per software package.

# The scripts

## Main Analyses

R_hierarchical_clustering.Rmd
- This script clusters the clusters resulting from a long-axis x age bin interaction into superclusters that share similar profiles of connectivity with the anteroposterior hippocampus (as visualized in Figure 4A). It also plots several cluster quality checks (including supplemental plots in Fig.S1.B-E)
- Input: Hippo_BinxAxis_F_betas.txt
- Output: supercluster solution (visualized in Figure 4A) and supplemental plots in Fig.S1.B-E

R_superclusters_leave1out.Rmd
- For leave1out crossvalidation purposes, this script provides 212 clustering solutions leaving a subject out each time
- Input: Hippo_BinxAxis_F_betas.txt and subj_list_n212.txt
- Output: clusters_leave1out.txt
  - cluster assignment for ROI x nsubject iterations

ClustRobust.m
- calculates the consistency matrix, where 100% clustered together
- Input: clusters_leave1out.txt
- Output: threshMat.csv and adjMat.csv

R_superclusters_stats.Rmd
- This script calculates a weighted average of data clustered together 100% consistently across leave1out iterations, and runs the statistical comparisons for each supercluster. It plots the supercluster connectivity profiles in Figure 4. It also runs the statistical analyses and plots for inside vs outside infantile amnesia window comparisons (Figure 5)
- Input: threshMat.csv, Hippo_BinxAxis_F_betas.txt, supercluster_antpost_betas_by_age.txt
- Output: Figures 4 and 5 plots, statistical comparisons

## Supplemental Analyses and Figures

supplemental_plot_consistency_matrices.Rmd
- This script creates plots of the clustering consistency matrices
- Input: adjMat.csv and data_cluster_labels.csv (output from ClustRobust.m and R_superclusters_stats.Rmd)
- Output: Fig. S1F matrices

supplemental_average_antpostdiff_profiles.Rmd
- This script selectively averages and plots anterior-posterior difference data
- Input: threshMat.csv and Hippo_BinxAxis_F_betas.txt
- Output: Fig. S5C, Fig. S6C, Fig. S7C, Fig. S8B, Fig S9B, and Fig. S10B

supplemental_individual_clust_profiles.Rmd
- This script plots the connectivity profiles for individual clusters that comprise the superclusters. See "Neocortical Cluster Label" column of Table S1 for information on cluster numbers.
- Input: Hippo_BinxAxis_F_betas.txt
- Output: individual cluster profile for whichever desired cluster. Figs S5-10 D panels.

supplemental_HemxLongAxisxAgeBin.Rmd
- This script plots hippocampal connectivity with neocortical clusters identified by a significant hemisphere x long-axis x age bin interaction.
- Input: Hippo_BinxHemxAxis_F_betas.txt
- Output: Plots in Fig S2C

supplemental_HemxLongAxis.Rmd
- This script plots hippocampal connectivity with neocortical clusters identified by a significant hemisphere x long-axis interaction.
- Input: Hippo_AxisxHem_F_betas.txt
- Output: Plots in Fig S3C

supplemental_HemxAgeBin.Rmd
- This script plots hippocampal connectivity with neocortical clusters identified by a significant hemisphere x age-bin interaction.
- Input: Hippo_BinxHem_F_betas.txt
- Output: Plots in Fig S4C

supplemental_tSNR_analyses.Rmd
- This script plots mean and standard deviation of connectivity across the age bins examined.
- Input: Mean_and_SD_cortical_conn_Schaefer2018_200Parcels.csv
- Output: Figs S12 and S13

supplemental_AgeDist.Rmd
- This script plots the distribution of ages examined.
- Input: babyhippos_demos.csv
- Output: Fig. S14

# The data
## Data derivatives
subj_list_n212.txt
- list of participants

babyhippos_demos.csv
- subj: participant number
- age: age in months
- tsnr: tSNR averaged across the brain
- diffmag: mean diffmag, a measure of motion calculated by AFNI: https://afni.nimh.nih.gov/pub/dist/doc/program_help/@1dDiffMag.html
- site: UMN= University of Minnesota, UNC= University of North Carolina, NoSite: Site not documented

Hippo_BinxAxis_F_betas.txt
- subj: subject number
- hem: hemisphere. left=left hemisphere, right= right hemisphere
- ax: hippocampal long-axis. ant= anterior hippocampus, post= posterior hippocampus
- roi: region of interest. hipp= hippocampus
- bin: age bin. 1: 0-6 months, 2: 7-12 months, 3: 13-18 months, 4: 19-25 months
- clust: cluster number for each significant cluster surviving an age bin x long-axis interaction at q=0.01 corrected across the whole brain. Note, the cluster threshold is not applied here. Only the first 44 clusters were used in the main manuscript, to get rid of very small clusters with less than 10 voxels (i.e. cluster threshold = 10).
- beta: connectivity values (Fisher z)

Hippo_BinxHemxAxis_F_betas.txt
- subj: subject number
- hem: hemisphere. left=left hemisphere, right= right hemisphere
- ax: hippocampal long-axis. ant= anterior hippocampus, post= posterior hippocampus
- roi: region of interest. hipp= hippocampus
- bin: age bin. 1: 0-6 months, 2: 7-12 months, 3: 13-18 months, 4: 19-25 months
- clust: cluster number for each significant cluster surviving an age bin x hemisphere x long-axis interaction. The top 4 clusters survive a cluster threshold of 10.
- beta: connectivity values (Fisher z)

Hippo_BinxHem_F_betas.txt
- subj: subject number
- hem: hemisphere. left=left hemisphere, right= right hemisphere
- ax: hippocampal long-axis. ant= anterior hippocampus, post= posterior hippocampus
- roi: region of interest. hipp= hippocampus
- bin: age bin. 1: 0-6 months, 2: 7-12 months, 3: 13-18 months, 4: 19-25 months
- clust: cluster number for each significant cluster surviving an age bin x hemisphere  interaction. Note, the cluster threshold is not applied here. Only the first 3 clusters survive a cluster threshold of 10.
- beta: connectivity values (Fisher z)

Hippo_AxisxHem_F_betas.txt
- subj: subject number
- hem: hemisphere. left=left hemisphere, right= right hemisphere
- ax: hippocampal long-axis. ant= anterior hippocampus, post= posterior hippocampus
- roi: region of interest. hipp= hippocampus
- bin: age bin. 1: 0-6 months, 2: 7-12 months, 3: 13-18 months, 4: 19-25 months
- clust: cluster number for each significant cluster surviving a long-axis x hemisphere  interaction. Note, the cluster threshold is not applied here. 12 clusters survive a cluster threshold of 10. 1 was excluded for being located in the cerebellum, and 2 for being located in white matter, leaving 9 reported in supplementary material.
- beta: connectivity values (Fisher z)

Mean_and_SD_cortical_conn_Schaefer2018_200Parcels.csv
- subj: participant
- age: age in months
- mean: mean connectivity across neocortical parcels
- sd: standard deviation of connectivity across neocortical parcels
- mean_tSNR_wb: whole brain mean tSNR
- mean_DiffMag: mean diffmag, a measure of motion calculated by AFNI: https://afni.nimh.nih.gov/pub/dist/doc/program_help/@1dDiffMag.html
- site: UMN= University of Minnesota, UNC= University of North Carolina, NoSite: Site not documented
- Bin: age bin. 1: 0-6 months, 2: 7-12 months, 3: 13-18 months, 4: 19-25 months, 5: 36-39 months

### R output data
data_cluster_labels.csv
- data_clust.clust: the cluster number resulting from the long-axis x age-bin interaction
- clusters: supercluster number each cluster is assigned to

clusters_leave1out.txt
- cluster x clustering solution across 212 iterations leaving one subject out each time

adjMat.csv
- adjacency matrix showing proportion of times regions were clustered together across leave-one-out iterations

threshMat.csv
- binarized consistency matrix, 1= the regions were clustered together 100% of the time across leave-one-out iterations

mod1_weighted_ax_bin_supercluster.rda
- R model of the interaction between long-axis, age bin, and supercluster. It takes a while to run, or you can read in this saved version. Model call:
~~~
model<-lme(beta ~ ax*bin*supercluster, random= ~1|subj, weights=varIdent(form=~1|bin*ax*supercluster), data=data_SC_all, control = lmeControl(msMaxIter=1000, msMaxEval=1000), na.action=na.omit)
~~~

## Raw neuroimaging data
This work utilizes data collected as part of the UNC/UMN Baby Connectome Project Consortium. Data used in the preparation of this manuscript were obtained from the NIMH Data Archive (NDA). NDA is a collaborative informatics system created by the National Institutes of Health to provide a national resource to support and accelerate research in mental health. Dataset identifier: 10.15154/w07p-s888. The associated manuscript reflects the views of the authors and may not reflect the opinions or views of the NIH or of the Submitters submitting original data to NDA.

# Running the scripts
To run these scripts you will need to download and save the scripts and data to your local machine. You will also need to install the software and R packages detailed above.

You will need to change the path in each script to point to wherever you saved the data and scripts on your local machine (denoted by ##### CHANGE PATH within each script)

Running these scripts will reproduce the statistical analyses and figures in the main manuscript and supplemental material. Each script should take no more than a few minutes to run.

Note that the R_output_data are the outputs of the R scripts above that are needed to run subsequent scripts. These are included so that you can run any of the scripts above without having to run them in order.

# License
All code in this repository is licensed under the MIT license.

Please see the NDA for the baby connectome project for data licensing information: 10.15154/w07p-s888

# Inquiries
Please contact samanthaaudrain at gmail dot com for questions, comments, or bugs.
