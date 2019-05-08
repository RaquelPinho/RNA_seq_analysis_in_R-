---
title: "RUV_Salmon_DESeq2_EdgeR_with_outliers"
author: "Raquel Pinho"
date: "1 de maio de 2019"
output: 
  html_document:
        toc: true
        toc_depth: 2
        toc_float: 
              collapsed: false
        number_sections: true
        fig.width: 11
        fig.height: 9
        fig.caption: true
        dev: png
        df_print: paged
        code_folding: hide
        self_contained: false
        keep_md: true
  word_document: default
editor_options: 
  chunk_output_type: inline
---

# Data

This code was writen using data from mhlz experiment 2016, where Malnourished and full fed animals were compared after the ingestion of regular milk, lysozyme-rich milk or no milk. The expression along the length of the intestinal tract were assessed by underlying distribution of RNA-seq data. In total we have seven groups and 4 or three animals per group.

I will first set the working directory to where output are going to be saved and choose the groups to be compared if it was not defined on the command line in the server, besides setting up the knitr options:


```r
#args = commandArgs(trailingOnly=TRUE)
setwd("C:/Users/rpmello/Desktop/Data Analysis RNASeq/Projeto_Salmon_DESeq_EdgeR")
#groups <- c(args[1],args[2])
#groups<-c("FFI","No_milkI")#colocar em ordem alfabética e com a inicial da seção
dir.create(file.path("C:/Users/rpmello/Desktop/Data Analysis RNASeq/Projeto_Salmon_DESeq_EdgeR/Output", sprintf("%s_vs_%s_Salmon_R_with_outliers",groups[1], groups[2])), showWarnings = FALSE)
output_dir<-file.path("C:/Users/rpmello/Desktop/Data Analysis RNASeq/Projeto_Salmon_DESeq_EdgeR/Output", sprintf("%s_vs_%s_Salmon_R_with_outliers",groups[1], groups[2]))
dir.create(file.path(output_dir, "Figures"), showWarnings = FALSE)
sprintf("Comparison of %s and %s.", groups[1], groups[2])
```

```
## [1] "Comparison of FFD and Goat_milkD."
```

```r
knitr::opts_chunk$set(echo = TRUE, results = "asis", include = TRUE, fig.path = file.path(output_dir,"Figures"), fig.width = 11, fig.height = 8, fig.show = "animated")
```

# DESeq2 analysis

I will follow the DESeq2 tutorial found [here](https://github.com/mikelove/asthma) and [here](Angus_workshop)

And I will start creating a table of counts with all the samples.But for this I will have to import the counts data and the metadata.

## Importing the data 

The data used was the quant.sf files output from the Salmon software. And I will select the data and the metadata to the groups selected for comparison.



## Importing and creating the metadata table



The metadata for the specified comparison was:


```r
head(selectMetadata)
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":[""],"name":["_rn_"],"type":[""],"align":["left"]},{"label":["Pig"],"name":[1],"type":["fctr"],"align":["left"]},{"label":["Description"],"name":[2],"type":["fctr"],"align":["left"]},{"label":["SampleID"],"name":[3],"type":["fctr"],"align":["left"]},{"label":["list_of_files"],"name":[4],"type":["fctr"],"align":["left"]},{"label":["Index"],"name":[5],"type":["int"],"align":["right"]},{"label":["Group"],"name":[6],"type":["fctr"],"align":["left"]},{"label":["Group_sectio"],"name":[7],"type":["fctr"],"align":["left"]},{"label":["Trial"],"name":[8],"type":["fctr"],"align":["left"]},{"label":["Time"],"name":[9],"type":["chr"],"align":["left"]},{"label":["Section"],"name":[10],"type":["fctr"],"align":["left"]},{"label":["Diet"],"name":[11],"type":["fctr"],"align":["left"]},{"label":["Fed"],"name":[12],"type":["fctr"],"align":["left"]},{"label":["Ecoli"],"name":[13],"type":["fctr"],"align":["left"]},{"label":["SectionTimeFed"],"name":[14],"type":["fctr"],"align":["left"]},{"label":["TimeSectionFedChal"],"name":[15],"type":["fctr"],"align":["left"]},{"label":["TRT"],"name":[16],"type":["fctr"],"align":["left"]},{"label":["Collection"],"name":[17],"type":["fctr"],"align":["left"]},{"label":["RNA_ext"],"name":[18],"type":["fctr"],"align":["left"]},{"label":["Lib_const"],"name":[19],"type":["fctr"],"align":["left"]},{"label":["Suceptability"],"name":[20],"type":["fctr"],"align":["left"]},{"label":["Lane"],"name":[21],"type":["fctr"],"align":["left"]},{"label":["Weight_3Week"],"name":[22],"type":["dbl"],"align":["right"]},{"label":["..22"],"name":[23],"type":["lgl"],"align":["right"]},{"label":["RIN"],"name":[24],"type":["dbl"],"align":["right"]},{"label":["Final_Weight"],"name":[25],"type":["dbl"],"align":["right"]},{"label":["MLNT"],"name":[26],"type":["dbl"],"align":["right"]},{"label":["mean_hist_crypt_value"],"name":[27],"type":["dbl"],"align":["right"]},{"label":["mean_hist_height_value"],"name":[28],"type":["dbl"],"align":["right"]},{"label":["mean_hist_LP_value"],"name":[29],"type":["dbl"],"align":["right"]},{"label":["mean_hist_width_value"],"name":[30],"type":["dbl"],"align":["right"]},{"label":["Small_weight"],"name":[31],"type":["int"],"align":["right"]},{"label":["Small_body_ratio"],"name":[32],"type":["dbl"],"align":["right"]},{"label":["Bacteriodetes"],"name":[33],"type":["dbl"],"align":["right"]},{"label":["Firmicutes"],"name":[34],"type":["dbl"],"align":["right"]},{"label":["bac_firm_ratio"],"name":[35],"type":["dbl"],"align":["right"]},{"label":["wt_lt_ratio"],"name":[36],"type":["dbl"],"align":["right"]},{"label":["z_score_animal"],"name":[37],"type":["dbl"],"align":["right"]},{"label":["z_score_group"],"name":[38],"type":["dbl"],"align":["right"]},{"label":["Litter"],"name":[39],"type":["fctr"],"align":["left"]},{"label":["Conductance"],"name":[40],"type":["dbl"],"align":["right"]},{"label":["Paracellular_permeability"],"name":[41],"type":["dbl"],"align":["right"]},{"label":["Transcellular_permeability"],"name":[42],"type":["dbl"],"align":["right"]},{"label":["Labels"],"name":[43],"type":["chr"],"align":["left"]}],"data":[{"1":"33-12","2":"33-12D","3":"index3L2","4":"index3L2_salmon_trim_quant.counts","5":"3","6":"FF","7":"FFD","8":"mhLZ","9":"Wk5","10":"Duodenum","11":"FF","12":"No","13":"No","14":"Duod_5FF","15":"Wk5_Duod_FFNo","16":"5FF","17":"6_17A","18":"4","19":"8_25","20":"NA","21":"2","22":"9.0","23":"NA","24":"7.7","25":"16.6","26":"75528.7009","27":"401.587","28":"502.540","29":"561.960","30":"192.466","31":"1542","32":"92.89157","33":"0.023587493","34":"0.6593527","35":"3.5773710","36":"0.2178478","37":"-0.9111080","38":"0.000000","39":"33","40":"6.760","41":"183.2455","42":"-2.421892","43":"FF1","_rn_":"FF1"},{"1":"6-10","2":"6-10D","3":"index5L5","4":"index5L5_salmon_trim_quant.counts","5":"5","6":"FF","7":"FFD","8":"mhLF","9":"Wk5","10":"Duodenum","11":"FF","12":"No","13":"No","14":"Duod_5FF","15":"Wk5_Duod_FFNo","16":"5FF","17":"4_3","18":"5","19":"8_25","20":"NA","21":"5","22":"10.5","23":"NA","24":"8.8","25":"17.1","26":"451500.0000","27":"473.050","28":"418.754","29":"550.390","30":"146.640","31":"1200","32":"70.17544","33":"0.006582556","34":"0.7794844","35":"0.8444757","36":"0.2172808","37":"-0.9426755","38":"0.000000","39":"6","40":"6.690","41":"668.5889","42":"31.963960","43":"FF2","_rn_":"FF2"},{"1":"8-11","2":"8-11D","3":"index6L6","4":"index6L6_salmon_trim_quant.counts","5":"6","6":"FF","7":"FFD","8":"mhLF","9":"Wk5","10":"Duodenum","11":"FF","12":"No","13":"No","14":"Duod_5FF","15":"Wk5_Duod_FFNo","16":"5FF","17":"4_3","18":"6","19":"8_25","20":"NA","21":"6","22":"13.1","23":"NA","24":"8.5","25":"19.9","26":"18000.0000","27":"246.565","28":"404.482","29":"449.008","30":"118.476","31":"1300","32":"65.32663","33":"0.014810752","34":"0.9402084","35":"1.5752625","36":"0.2447724","37":"0.5880298","38":"0.000000","39":"8","40":"6.689","41":"392.7977","42":"177.221600","43":"FF3","_rn_":"FF3"},{"1":"9-3","2":"9-3D","3":"index2L1","4":"index2L1_salmon_trim_quant.counts","5":"2","6":"FF","7":"FFD","8":"mhLF","9":"Wk5","10":"Duodenum","11":"FF","12":"No","13":"No","14":"Duod_5FF","15":"Wk5_Duod_FFNo","16":"5FF","17":"4_4","18":"6","19":"8_25","20":"NA","21":"1","22":"14.1","23":"NA","24":"9.1","25":"22.2","26":"31500.0000","27":"279.575","28":"580.830","29":"568.920","30":"190.026","31":"1300","32":"58.55856","33":"0.004936917","34":"0.9522765","35":"0.5184332","36":"0.2569444","37":"1.2657538","38":"0.000000","39":"9","40":"5.177","41":"177.4162","42":"19.287990","43":"FF4","_rn_":"FF4"},{"1":"29-13","2":"29-13D","3":"index4L3","4":"index4L3_salmon_trim_quant.counts","5":"4","6":"Goat_milk","7":"Goat_milkD","8":"mhLZ","9":"Wk5","10":"Duodenum","11":"Mal","12":"Goat","13":"No","14":"Duod_5MalGoat","15":"Wk5_Duod_MalGoat","16":"MalGoat","17":"6_17P","18":"1","19":"8_25","20":"NA","21":"3","22":"6.5","23":"NA","24":"9.0","25":"8.3","26":"743.3102","27":"292.918","28":"320.002","29":"325.646","30":"159.796","31":"454","32":"54.69880","33":"0.010422381","34":"0.7827756","35":"1.3314646","36":"0.1307087","37":"-5.7629220","38":"-4.131395","39":"29","40":"18.890","41":"2139.4460","42":"199.802400","43":"Goat_milk1","_rn_":"Goat_milk1"},{"1":"31-13","2":"31-13D","3":"index7L6","4":"index7L6_salmon_trim_quant.counts","5":"7","6":"Goat_milk","7":"Goat_milkD","8":"mhLZ","9":"Wk5","10":"Duodenum","11":"Mal","12":"Goat","13":"No","14":"Duod_5MalGoat","15":"Wk5_Duod_MalGoat","16":"MalGoat","17":"6_16A","18":"3","19":"10_8","20":"NA","21":"6","22":"9.2","23":"NA","24":"8.6","25":"12.6","26":"5136.9863","27":"229.253","28":"453.530","29":"339.776","30":"185.772","31":"635","32":"50.39683","33":"0.012616566","34":"0.6302798","35":"2.0017406","36":"0.1836735","37":"-2.8138971","38":"-4.131395","39":"31","40":"9.830","41":"950.6824","42":"165.317300","43":"Goat_milk2","_rn_":"Goat_milk2"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

### Removing dependent variables

I will remove the metadata that does not present a variation in this comparison or have all unique values in this comparison


```r
batch_effects<-sapply(1:ncol(selectMetadata), function(i){length(unique(selectMetadata[,i]))!= 1 & length(unique(selectMetadata[,i])) != length(selectMetadata[,i])})
batch_effects<-selectMetadata[,batch_effects]
dependent<-sapply(1:ncol(batch_effects), function(i){length(unique(batch_effects[1:4,i])) == 1 & length(unique(batch_effects[5:8,i])) == 1})
independent<-which(!dependent)
batch_effects<-batch_effects[,independent]
batch_effects$Group<-group
batch_effects
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":[""],"name":["_rn_"],"type":[""],"align":["left"]},{"label":["Index"],"name":[1],"type":["int"],"align":["right"]},{"label":["Trial"],"name":[2],"type":["fctr"],"align":["left"]},{"label":["Collection"],"name":[3],"type":["fctr"],"align":["left"]},{"label":["RNA_ext"],"name":[4],"type":["fctr"],"align":["left"]},{"label":["Lib_const"],"name":[5],"type":["fctr"],"align":["left"]},{"label":["Lane"],"name":[6],"type":["fctr"],"align":["left"]},{"label":["RIN"],"name":[7],"type":["dbl"],"align":["right"]},{"label":["Small_weight"],"name":[8],"type":["int"],"align":["right"]},{"label":["Bacteriodetes"],"name":[9],"type":["dbl"],"align":["right"]},{"label":["Litter"],"name":[10],"type":["fctr"],"align":["left"]},{"label":["Group"],"name":[11],"type":["fctr"],"align":["left"]}],"data":[{"1":"3","2":"mhLZ","3":"6_17A","4":"4","5":"8_25","6":"2","7":"7.7","8":"1542","9":"0.023587493","10":"33","11":"FF","_rn_":"FF1"},{"1":"5","2":"mhLF","3":"4_3","4":"5","5":"8_25","6":"5","7":"8.8","8":"1200","9":"0.006582556","10":"6","11":"FF","_rn_":"FF2"},{"1":"6","2":"mhLF","3":"4_3","4":"6","5":"8_25","6":"6","7":"8.5","8":"1300","9":"0.014810752","10":"8","11":"FF","_rn_":"FF3"},{"1":"2","2":"mhLF","3":"4_4","4":"6","5":"8_25","6":"1","7":"9.1","8":"1300","9":"0.004936917","10":"9","11":"FF","_rn_":"FF4"},{"1":"4","2":"mhLZ","3":"6_17P","4":"1","5":"8_25","6":"3","7":"9.0","8":"454","9":"0.010422381","10":"29","11":"Goat_milk","_rn_":"Goat_milk1"},{"1":"7","2":"mhLZ","3":"6_16A","4":"3","5":"10_8","6":"6","7":"8.6","8":"635","9":"0.012616566","10":"31","11":"Goat_milk","_rn_":"Goat_milk2"},{"1":"5","2":"mhLZ","3":"6_16P","4":"2","5":"8_27","6":"4","7":"9.1","8":"635","9":"0.012616566","10":"31","11":"Goat_milk","_rn_":"Goat_milk3"},{"1":"6","2":"mhLZ","3":"6_16A","4":"2","5":"10_8","6":"5","7":"9.1","8":"544","9":"0.079539221","10":"31","11":"Goat_milk","_rn_":"Goat_milk4"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

From now on I will use the manual from [RUVSeq](https://bioconductor.org/packages/release/bioc/vignettes/RUVSeq/inst/doc/RUVSeq.pdf) and the manual from [DESeq2](https://www.bioconductor.org/packages/devel/workflows/vignettes/rnaseqGene/inst/doc/rnaseqGene.html#using-ruv-with-deseq2)

## DESeq object

Creating a DESeqDataSet for the in this case we will use the design ~Group, because, for know we are only interested in the normalized counts to filter low counts and calculate the variation varible with RUV.  



```r
library(DESeq2)
dds <-DESeqDataSetFromTximport(txidata, batch_effects, as.formula("~Group"))
```

```
## factor levels were dropped which had no samples
```

```
## using counts and average transcript lengths from tximport
```

```r
#It doesn't matter the formula now, because I am just interested in the counts, but I 
dds<-estimateSizeFactors(dds)
```

```
## using 'avgTxLength' from assays(dds), correcting for library size
```

```r
dds<-DESeq(dds)
```

```
## using pre-existing normalization factors
```

```
## estimating dispersions
```

```
## gene-wise dispersion estimates
```

```
## mean-dispersion relationship
```

```
## final dispersion estimates
```

```
## fitting model and testing
```

### Filtering low expressed genes

I wil remove the genes if low counts, and I am being very stringent here, removing the ones tha have lower than 20 counts in all the samples and the ones that have less then 5 counts in at least two samples. 

I will use the [EDASeq](http://bioconductor.org/packages/release/bioc/manuals/EDASeq/man/EDASeq.pdf) package just to create an object were I can store the data and the metadata for multiple analysis.


```r
library(EDASeq)
library(RColorBrewer)
#creating a set with the expression dat
set<-newSeqExpressionSet(DESeq2::counts(dds), phenoData= batch_effects)
set
```

SeqExpressionSet (storageMode: lockedEnvironment)
assayData: 22630 features, 8 samples 
  element names: counts, normalizedCounts, offset 
protocolData: none
phenoData
  sampleNames: FF1 FF2 ... Goat_milk4 (8 total)
  varLabels: Index Trial ... Group (11 total)
  varMetadata: labelDescription
featureData: none
experimentData: use 'experimentData(object)'
Annotation:  

```r
colors<-brewer.pal(4,"Set2")
idx  <- rowSums(EDASeq::counts(set) > 5) >= 2
set  <- set[idx, ]
filter<-apply(EDASeq::counts(set),1,function(x){length(x[x>5])>2})
set <-set[filter,]
sprintf("Removed %s genes with low counts from %s total genes", nrow(EDASeq::counts(dds))-nrow(EDASeq::counts(set)),nrow(DESeq2::counts(dds)))
```

[1] "Removed 6230 genes with low counts from 22630 total genes"

## RUVSeq

The number of sig DE genes with mod1 (~Group) was:


```r
res<-results(dds)
sigres<-res[!is.na(res$padj),]
sigres<-sigres[sigres$padj<0.1 & sigres$log2FoldChange>1| sigres$log2FoldChange< -1,]
dim(sigres)
```

[1] 1118    6

### Box plot of the counts per sample 

We can analyze the variation of the counts before normalization and reduced variation.


```r
plotRLE(set, outline =F, ylim=c(-2,2), col=colors[set$Group])
```

![](C:/Users/rpmello/Desktop/Data Analysis RNASeq/Projeto_Salmon_DESeq_EdgeR/Output/FFD_vs_Goat_milkD_Salmon_R_with_outliers/Figuresbox_plot_counts-1.png)

```r
plotPCA(set, col=colors[set$Group])
```

![](C:/Users/rpmello/Desktop/Data Analysis RNASeq/Projeto_Salmon_DESeq_EdgeR/Output/FFD_vs_Goat_milkD_Salmon_R_with_outliers/Figuresbox_plot_counts-2.png)
 
### Upper quartile normalization

The upper quartile normalization [(UQ)](https://bmcbioinformatics.biomedcentral.com/articles/10.1186/1471-2105-11-94) normalizes the data  accounting for the most highly expressed transcripts, since there is a bias on the number of reads  associated to them, i.e, more expresssed trancripts generate more reads and futhermore the number of reads on these highly expressed genes is proportional to the total number of reads in the sample (if samples are in individual lanes or in the same lane), which is not the case for me. [Here](http://vinaykmittal.blogspot.com/2013/10/fpkmrpkm-normalization-caveat-and-upper.html)a more human explanation. And [here](https://academic.oup.com/bib/article/14/6/671/189645) a more recent paper on RNA data normalization. 


```r
set_n<-betweenLaneNormalization(set,which = "upper")
set_n$Group<-factor(set_n$Group)
plotRLE(set_n, outline=F, col=colors[set_n$Group])
```

![](C:/Users/rpmello/Desktop/Data Analysis RNASeq/Projeto_Salmon_DESeq_EdgeR/Output/FFD_vs_Goat_milkD_Salmon_R_with_outliers/Figuresupper-1.png)

```r
plotPCA(set_n,col=colors[set_n$Group])
```

![](C:/Users/rpmello/Desktop/Data Analysis RNASeq/Projeto_Salmon_DESeq_EdgeR/Output/FFD_vs_Goat_milkD_Salmon_R_with_outliers/Figuresupper-2.png)

### Using empirical control genes 

Since no spike in were used we will use the genes that have least changed between samples ( least significantly DE genes ) and use them as controls for the unwanted factors. So first we do a DE analysis and find the genes and then we do the normalization. 

### Empirical controls using the prior DE analisis using DESeq2 

Now we are going to use the DESeq2 to do DE analysis so we can pick the least DE genes as controls.In this case I will estimate only one unwanted variacen variable. 


```r
#Using RUVSeq
library(RUVSeq)
not.sig <- rownames(res)[which(res$pvalue > .2)]
empirical <- rownames(set_n)[ rownames(set_n) %in% not.sig ]
if("RDAVIDWebService" %in% (.packages())){
  detach("package:RDAVIDWebService", unload=TRUE) 
}
set_n <- RUVg(set_n, empirical, k=1)
pData(set_n)
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":[""],"name":["_rn_"],"type":[""],"align":["left"]},{"label":["Index"],"name":[1],"type":["int"],"align":["right"]},{"label":["Trial"],"name":[2],"type":["fctr"],"align":["left"]},{"label":["Collection"],"name":[3],"type":["fctr"],"align":["left"]},{"label":["RNA_ext"],"name":[4],"type":["fctr"],"align":["left"]},{"label":["Lib_const"],"name":[5],"type":["fctr"],"align":["left"]},{"label":["Lane"],"name":[6],"type":["fctr"],"align":["left"]},{"label":["RIN"],"name":[7],"type":["dbl"],"align":["right"]},{"label":["Small_weight"],"name":[8],"type":["int"],"align":["right"]},{"label":["Bacteriodetes"],"name":[9],"type":["dbl"],"align":["right"]},{"label":["Litter"],"name":[10],"type":["fctr"],"align":["left"]},{"label":["Group"],"name":[11],"type":["fctr"],"align":["left"]},{"label":["W_1"],"name":[12],"type":["dbl"],"align":["right"]}],"data":[{"1":"3","2":"mhLZ","3":"6_17A","4":"4","5":"8_25","6":"2","7":"7.7","8":"1542","9":"0.023587493","10":"33","11":"FF","12":"-0.4227586","_rn_":"FF1"},{"1":"5","2":"mhLF","3":"4_3","4":"5","5":"8_25","6":"5","7":"8.8","8":"1200","9":"0.006582556","10":"6","11":"FF","12":"0.2468657","_rn_":"FF2"},{"1":"6","2":"mhLF","3":"4_3","4":"6","5":"8_25","6":"6","7":"8.5","8":"1300","9":"0.014810752","10":"8","11":"FF","12":"0.1754108","_rn_":"FF3"},{"1":"2","2":"mhLF","3":"4_4","4":"6","5":"8_25","6":"1","7":"9.1","8":"1300","9":"0.004936917","10":"9","11":"FF","12":"0.3064810","_rn_":"FF4"},{"1":"4","2":"mhLZ","3":"6_17P","4":"1","5":"8_25","6":"3","7":"9.0","8":"454","9":"0.010422381","10":"29","11":"Goat_milk","12":"0.1878971","_rn_":"Goat_milk1"},{"1":"7","2":"mhLZ","3":"6_16A","4":"3","5":"10_8","6":"6","7":"8.6","8":"635","9":"0.012616566","10":"31","11":"Goat_milk","12":"-0.5922968","_rn_":"Goat_milk2"},{"1":"5","2":"mhLZ","3":"6_16P","4":"2","5":"8_27","6":"4","7":"9.1","8":"635","9":"0.012616566","10":"31","11":"Goat_milk","12":"0.3989652","_rn_":"Goat_milk3"},{"1":"6","2":"mhLZ","3":"6_16A","4":"2","5":"10_8","6":"5","7":"9.1","8":"544","9":"0.079539221","10":"31","11":"Goat_milk","12":"-0.3005643","_rn_":"Goat_milk4"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

Using the unwanted variance variable in DESeq ( ~ W1 + Group).


```r
ddsruv <- dds
ddsruv$W1 <- set_n$W_1
design(ddsruv) <- ~ W1 + Group
dds<-ddsruv
```

## Check sample distances

We will use the `dist` function to calculate Euclidean distances bewteen samples and see with the sample within groups are similar. And in this case we will use the rlog transformed data so the high variance if the low expressed genes do not influence the calculations.

Poisson distance of the smples and euclidean of rlog transformed data.



The heatmap of the rlog transformed counts using poisson distance.


```r
colors<- colorRampPalette(rev(brewer.pal(9,"Blues")))(255)
hc<- hclust(poisd$dd)
heatmap.2(samplePoisDistMatrix, Rowv=as.dendrogram(hc), symm=T, trace="none", col= colors, margins=c(2,10), labCol=F)
```

![](C:/Users/rpmello/Desktop/Data Analysis RNASeq/Projeto_Salmon_DESeq_EdgeR/Output/FFD_vs_Goat_milkD_Salmon_R_with_outliers/Figuresheatmap_rlog_poisson-1.png)<!-- -->

We can also see the PCA plot:


```r
library(ggplot2)
z <-plotPCA(rlogdds, intgroup = c( "Group"))
z+ geom_text(aes(label = selectMetadata$Pig))
```

![](C:/Users/rpmello/Desktop/Data Analysis RNASeq/Projeto_Salmon_DESeq_EdgeR/Output/FFD_vs_Goat_milkD_Salmon_R_with_outliers/FiguresPCA_plot-1.png)

```r
# Using the both
percentVar <- round(100 * attr(z, "percentVar"))
ggplot(as.data.frame(z$data), aes(PC1, PC2, shape=Group, color=rlogdds$Collection)) +  geom_point(size=3) +
  xlab(paste0("PC1: ",percentVar[1],"% variance")) +
  ylab(paste0("PC2: ",percentVar[2],"% variance")) + 
  coord_fixed()+ geom_text(aes(label = selectMetadata$Pig))
```

![](C:/Users/rpmello/Desktop/Data Analysis RNASeq/Projeto_Salmon_DESeq_EdgeR/Output/FFD_vs_Goat_milkD_Salmon_R_with_outliers/FiguresPCA_plot-2.png)

## Differrential expression analaysis

It is convenient to make sure that FF is the first level in the Diet factor so the default log2 fold change can be calculatedmalnourished over full fed.

Now, we can run the DESeq function that will estimate the size factors (estimate for differences due to library size), estimate dispersion for each gene, and fit in a generalized linear model.


we have this number of genes with padj < 0.1 L2FC 1. the L2FC estimation is based on MLE (betaprior is False) and the last variable is the contrast that is used.  



The summary of the results  from the DE analysis using DESeq2, where the low counts were excluded from testing due to being lower than the mean count.


```r
summary(res)
```


out of 15838 with nonzero total read count
adjusted p-value < 0.1
LFC > 0 (up)       : 1403, 8.9%
LFC < 0 (down)     : 1323, 8.4%
outliers [1]       : 0, 0%
low counts [2]     : 922, 5.8%
(mean count < 17)
[1] see 'cooksCutoff' argument of ?results
[2] see 'independentFiltering' argument of ?results

```r
#the low counts in the summary is the number of genes that were excluded from testing because the countes were lower than the mean count calculated by DESeq.
# it will test the last variable from the design , 
# This will give me the genes that are significantly different with a adj. pvalue < 0.1.
# To make it more stringent we can use adj.pvalue < 0.05, and L2fc > 2, or < -2.
```

Now we can filter the results by p. adjusted and log2foldChange values.Here we used padj< 0.1, log2FoldChange < -1 or log2FoldChange > 1. The table and the dimentions of the table of DE genes were: 


```r
res.sort<- res[order(res$pvalue), ]
#head(res)
#Considering a padj < 0.1 being significant  we can subset the significant differentiated genes
#resSig_Salmon <- subset(res, padj< 0.1)
res<-as.data.frame(res)
resSig_Salmon <- res[res$padj< 0.1,]
resSig_Salmon <- subset(resSig_Salmon, log2FoldChange < -1 | log2FoldChange > 1)
saveRDS(resSig_Salmon, file = file.path(output_dir,sprintf("%s_vs_%s_Salmon_resSig_w_outlier.RDS",groups[1], groups[2])))
head(as.data.frame(resSig_Salmon))
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":[""],"name":["_rn_"],"type":[""],"align":["left"]},{"label":["baseMean"],"name":[1],"type":["dbl"],"align":["right"]},{"label":["log2FoldChange"],"name":[2],"type":["dbl"],"align":["right"]},{"label":["lfcSE"],"name":[3],"type":["dbl"],"align":["right"]},{"label":["stat"],"name":[4],"type":["dbl"],"align":["right"]},{"label":["pvalue"],"name":[5],"type":["dbl"],"align":["right"]},{"label":["padj"],"name":[6],"type":["dbl"],"align":["right"]}],"data":[{"1":"1496.098","2":"1.263119","3":"0.4655519","4":"2.713165","5":"6.664398e-03","6":"5.806434e-02","_rn_":"ENSSSCG00000000033"},{"1":"2227.107","2":"1.280683","3":"0.2925383","4":"4.377830","5":"1.198669e-05","6":"1.433625e-03","_rn_":"ENSSSCG00000000038"},{"1":"5224.937","2":"1.135182","3":"0.2807886","4":"4.042836","5":"5.280851e-05","6":"3.680802e-03","_rn_":"ENSSSCG00000000133"},{"1":"536.294","2":"1.736426","3":"0.4924211","4":"3.526303","5":"4.214051e-04","6":"1.342106e-02","_rn_":"ENSSSCG00000000206"},{"1":"1948.814","2":"1.807710","3":"0.2748258","4":"6.577659","5":"4.779136e-11","6":"7.920621e-08","_rn_":"ENSSSCG00000000214"},{"1":"76646.855","2":"1.147281","3":"0.3254468","4":"3.525248","5":"4.230858e-04","6":"1.342106e-02","_rn_":"ENSSSCG00000000252"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

```r
dim(resSig_Salmon)
```

[1] 858   6

### Increased strigency 
Being more stringent we can see the number of genes 
with padj< 0.1 and L2FC < -1.5 or > 1.5


```r
resSig_Salmon<-resSig_Salmon[order(resSig_Salmon$padj), ]
resSig_Salmon1.5 <- subset(resSig_Salmon, log2FoldChange < -1.5 | log2FoldChange > 1.5)
dim(resSig_Salmon1.5)
```

[1] 219   6

### Sorting by up regulated or down regulated genes 

First a look at the top downregulated genes (based on log2foldchange):


```r
# sorting by the Log2FoldChange
head(as.data.frame(resSig_Salmon[order(resSig_Salmon$log2FoldChange), ])) # stronger downregulation
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":[""],"name":["_rn_"],"type":[""],"align":["left"]},{"label":["baseMean"],"name":[1],"type":["dbl"],"align":["right"]},{"label":["log2FoldChange"],"name":[2],"type":["dbl"],"align":["right"]},{"label":["lfcSE"],"name":[3],"type":["dbl"],"align":["right"]},{"label":["stat"],"name":[4],"type":["dbl"],"align":["right"]},{"label":["pvalue"],"name":[5],"type":["dbl"],"align":["right"]},{"label":["padj"],"name":[6],"type":["dbl"],"align":["right"]}],"data":[{"1":"2233.39242","2":"-5.803692","3":"1.7338867","4":"-3.347215","5":"8.162774e-04","6":"0.0194188090","_rn_":"ENSSSCG00000001836"},{"1":"933.56714","2":"-5.709618","3":"1.4522076","4":"-3.931682","5":"8.435362e-05","6":"0.0049731958","_rn_":"ENSSSCG00000022724"},{"1":"955.62108","2":"-4.633049","3":"0.9290203","4":"-4.987027","5":"6.131544e-07","6":"0.0001693669","_rn_":"ENSSSCG00000037358"},{"1":"42.42273","2":"-3.954534","3":"0.8757009","4":"-4.515850","5":"6.306340e-06","6":"0.0009044747","_rn_":"ENSSSCG00000001844"},{"1":"38.95166","2":"-3.818095","3":"1.4220218","4":"-2.684976","5":"7.253494e-03","6":"0.0605392859","_rn_":"ENSSSCG00000033906"},{"1":"21.89045","2":"-3.721314","3":"0.8381495","4":"-4.439917","5":"8.999349e-06","6":"0.0011879140","_rn_":"ENSSSCG00000037090"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

```r
topdown_salmon<-rownames(head(resSig_Salmon[order(resSig_Salmon$log2FoldChange), ],10)) # stronger downregulation

#Getting the down regulated genes 
downresSig_Salmon<-resSig_Salmon[resSig_Salmon$log2FoldChange < -1,]
downresSig_Salmon<-downresSig_Salmon[order(downresSig_Salmon$log2FoldChange),]
dim(downresSig_Salmon)
```

[1] 450   6

And now at the upregulated genes:


```r
head(as.data.frame(resSig_Salmon[order(-resSig_Salmon$log2FoldChange), ])) #stronger upregulation
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":[""],"name":["_rn_"],"type":[""],"align":["left"]},{"label":["baseMean"],"name":[1],"type":["dbl"],"align":["right"]},{"label":["log2FoldChange"],"name":[2],"type":["dbl"],"align":["right"]},{"label":["lfcSE"],"name":[3],"type":["dbl"],"align":["right"]},{"label":["stat"],"name":[4],"type":["dbl"],"align":["right"]},{"label":["pvalue"],"name":[5],"type":["dbl"],"align":["right"]},{"label":["padj"],"name":[6],"type":["dbl"],"align":["right"]}],"data":[{"1":"6393.15468","2":"3.973568","3":"0.4443965","4":"8.941491","5":"3.839536e-19","6":"5.727052e-15","_rn_":"ENSSSCG00000010488"},{"1":"104.45056","2":"3.501165","3":"1.1625612","4":"3.011596","5":"2.598781e-03","6":"3.607114e-02","_rn_":"ENSSSCG00000014910"},{"1":"19174.31905","2":"3.483967","3":"0.4890347","4":"7.124171","5":"1.047088e-12","6":"3.904592e-09","_rn_":"ENSSSCG00000035347"},{"1":"65.71552","2":"3.149597","3":"0.5613114","4":"5.611141","5":"2.009970e-08","6":"1.199228e-05","_rn_":"ENSSSCG00000029754"},{"1":"871.02702","2":"3.096292","3":"0.8196573","4":"3.777545","5":"1.583818e-04","6":"7.499757e-03","_rn_":"ENSSSCG00000007436"},{"1":"313.72152","2":"2.967147","3":"0.8794670","4":"3.373801","5":"7.413789e-04","6":"1.833903e-02","_rn_":"ENSSSCG00000029592"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

```r
topup_salmon<-rownames(head(resSig_Salmon[order(-resSig_Salmon$log2FoldChange), ],10)) #stronger upregulation

#Getting the up regulated genes
upresSig_Salmon<-resSig_Salmon[resSig_Salmon$log2FoldChange > 1,]
upresSig_Salmon<-upresSig_Salmon[order(-upresSig_Salmon$log2FoldChange),]
dim(upresSig_Salmon)
```

[1] 408   6


#### List of genes with padj < 0.1, L2FC < -1 or > 1.

Now we can have the list of the genes that are DE expressed and down or up regulated and then convert the ensembl ids into gene names using BiomaRt.  

The number of downregulated genes with successful convertion and the top 30 downregulated (based on log2folfchange) genes were:


```r
library(biomaRt)
mart<- useMart("ensembl")
Pig_ensembl<- useDataset("sscrofa_gene_ensembl", mart=mart)
Human<-useDataset("hsapiens_gene_ensembl", mart=mart)
down_genes_names<- getBM(attributes = c("ensembl_gene_id","external_gene_name"), filters= "ensembl_gene_id", values = rownames(downresSig_Salmon), mart = Pig_ensembl)
down_genes_names<-transform(merge(as.data.frame(downresSig_Salmon),down_genes_names,by.x=0, by.y ="ensembl_gene_id",all=TRUE), row.names=Row.names, Row.names=NULL)
down_genes_names_Salmon<- down_genes_names[order(down_genes_names$log2FoldChange),]
#The number of genes  we were able to find the gene name on BiomaRt
sum(down_genes_names_Salmon$external_gene_name != "")
```

[1] 338

```r
head(down_genes_names_Salmon[order(down_genes_names_Salmon$padj),], 30)
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":[""],"name":["_rn_"],"type":[""],"align":["left"]},{"label":["baseMean"],"name":[1],"type":["dbl"],"align":["right"]},{"label":["log2FoldChange"],"name":[2],"type":["dbl"],"align":["right"]},{"label":["lfcSE"],"name":[3],"type":["dbl"],"align":["right"]},{"label":["stat"],"name":[4],"type":["dbl"],"align":["right"]},{"label":["pvalue"],"name":[5],"type":["dbl"],"align":["right"]},{"label":["padj"],"name":[6],"type":["dbl"],"align":["right"]},{"label":["external_gene_name"],"name":[7],"type":["chr"],"align":["left"]}],"data":[{"1":"135.40372","2":"-2.460382","3":"0.3415207","4":"-7.204197","5":"5.838679e-13","6":"3.904592e-09","7":"","_rn_":"ENSSSCG00000032158"},{"1":"129.72591","2":"-3.058390","3":"0.4586516","4":"-6.668220","5":"2.589251e-11","6":"5.517325e-08","7":"","_rn_":"ENSSSCG00000028962"},{"1":"64.64559","2":"-1.900560","3":"0.3109723","4":"-6.111669","5":"9.859428e-10","6":"1.050452e-06","7":"NEURL3","_rn_":"ENSSSCG00000008124"},{"1":"517.14899","2":"-3.261102","3":"0.5516975","4":"-5.911034","5":"3.399677e-09","6":"2.817199e-06","7":"SLC16A9","_rn_":"ENSSSCG00000010210"},{"1":"209.83512","2":"-1.618131","3":"0.2771459","4":"-5.838553","5":"5.265616e-09","6":"3.740091e-06","7":"","_rn_":"ENSSSCG00000037679"},{"1":"357.93996","2":"-1.130637","3":"0.1999358","4":"-5.654997","5":"1.558491e-08","6":"1.010715e-05","7":"","_rn_":"ENSSSCG00000037695"},{"1":"120.14163","2":"-3.124953","3":"0.5896791","4":"-5.299413","5":"1.161756e-07","6":"4.951071e-05","7":"CA3","_rn_":"ENSSSCG00000006141"},{"1":"46.60635","2":"-2.430922","3":"0.4592887","4":"-5.292797","5":"1.204596e-07","6":"4.991044e-05","7":"","_rn_":"ENSSSCG00000001232"},{"1":"39.79892","2":"-3.711527","3":"0.7227608","4":"-5.135207","5":"2.818331e-07","6":"9.736374e-05","7":"GTSF1","_rn_":"ENSSSCG00000026333"},{"1":"250.75079","2":"-1.300940","3":"0.2535129","4":"-5.131653","5":"2.872087e-07","6":"9.736374e-05","7":"FAM111B","_rn_":"ENSSSCG00000013147"},{"1":"536.63737","2":"-1.290900","3":"0.2512241","4":"-5.138439","5":"2.770305e-07","6":"9.736374e-05","7":"","_rn_":"ENSSSCG00000034665"},{"1":"353.70105","2":"-1.592171","3":"0.3120998","4":"-5.101480","5":"3.370079e-07","6":"1.069534e-04","7":"","_rn_":"ENSSSCG00000036442"},{"1":"955.62108","2":"-4.633049","3":"0.9290203","4":"-4.987027","5":"6.131544e-07","6":"1.693669e-04","7":"","_rn_":"ENSSSCG00000037358"},{"1":"710.49324","2":"-1.423789","3":"0.2863926","4":"-4.971457","5":"6.645164e-07","6":"1.802169e-04","7":"EPSTI1","_rn_":"ENSSSCG00000037572"},{"1":"284.33425","2":"-1.471710","3":"0.2979063","4":"-4.940178","5":"7.805137e-07","6":"2.008574e-04","7":"CEP55","_rn_":"ENSSSCG00000010477"},{"1":"253.06678","2":"-1.023621","3":"0.2086457","4":"-4.906025","5":"9.294057e-07","6":"2.235970e-04","7":"","_rn_":"ENSSSCG00000021069"},{"1":"799.39934","2":"-2.118215","3":"0.4366965","4":"-4.850543","5":"1.231241e-06","6":"2.700764e-04","7":"NOS2","_rn_":"ENSSSCG00000017755"},{"1":"938.56025","2":"-1.789891","3":"0.3719279","4":"-4.812467","5":"1.490784e-06","6":"3.176648e-04","7":"","_rn_":"ENSSSCG00000014618"},{"1":"247.47887","2":"-1.485828","3":"0.3109857","4":"-4.777803","5":"1.772211e-06","6":"3.524574e-04","7":"CXCL2","_rn_":"ENSSSCG00000008959"},{"1":"968.56167","2":"-1.088607","3":"0.2293026","4":"-4.747467","5":"2.059797e-06","6":"4.042623e-04","7":"","_rn_":"ENSSSCG00000037083"},{"1":"421.40572","2":"-2.425177","3":"0.5134960","4":"-4.722875","5":"2.325340e-06","6":"4.366397e-04","7":"CXCL9","_rn_":"ENSSSCG00000032777"},{"1":"184.42438","2":"-1.457641","3":"0.3087283","4":"-4.721435","5":"2.341859e-06","6":"4.366397e-04","7":"CD274","_rn_":"ENSSSCG00000005211"},{"1":"550.28283","2":"-1.416025","3":"0.3044524","4":"-4.651054","5":"3.302436e-06","6":"5.597629e-04","7":"GZMA","_rn_":"ENSSSCG00000016903"},{"1":"774.59925","2":"-1.243688","3":"0.2728178","4":"-4.558675","5":"5.147741e-06","6":"7.678370e-04","7":"CXCL8","_rn_":"ENSSSCG00000008953"},{"1":"42.42273","2":"-3.954534","3":"0.8757009","4":"-4.515850","5":"6.306340e-06","6":"9.044747e-04","7":"PLIN1","_rn_":"ENSSSCG00000001844"},{"1":"242.88434","2":"-1.462294","3":"0.3240973","4":"-4.511899","5":"6.424985e-06","6":"9.077094e-04","7":"JAKMIP1","_rn_":"ENSSSCG00000008710"},{"1":"263.67991","2":"-2.253168","3":"0.5024914","4":"-4.483994","5":"7.325880e-06","6":"1.011785e-03","7":"OASL","_rn_":"ENSSSCG00000009921"},{"1":"21.89045","2":"-3.721314","3":"0.8381495","4":"-4.439917","5":"8.999349e-06","6":"1.187914e-03","7":"NEU4","_rn_":"ENSSSCG00000037090"},{"1":"948.84718","2":"-1.101462","3":"0.2524811","4":"-4.362554","5":"1.285526e-05","6":"1.521818e-03","7":"HSPA4L","_rn_":"ENSSSCG00000009077"},{"1":"715.12034","2":"-1.469202","3":"0.3377974","4":"-4.349359","5":"1.365361e-05","6":"1.566594e-03","7":"","_rn_":"ENSSSCG00000034560"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

For the upregulated: 


```r
up_genes_names<- getBM(attributes = c("ensembl_gene_id","external_gene_name"), filters= "ensembl_gene_id", values = rownames(upresSig_Salmon), mart = Pig_ensembl)
up_genes_names<-transform(merge(as.data.frame(upresSig_Salmon),up_genes_names,by.x=0, by.y ="ensembl_gene_id",all=TRUE), row.names=Row.names, Row.names=NULL)
up_genes_names_Salmon<- up_genes_names[order(up_genes_names$log2FoldChange),]
sum(up_genes_names_Salmon$external_gene_name != "")
```

[1] 326

```r
#The number of genes  we were able to find the gene name on BiomaRt
head(up_genes_names_Salmon[order(up_genes_names_Salmon$padj),],30)
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":[""],"name":["_rn_"],"type":[""],"align":["left"]},{"label":["baseMean"],"name":[1],"type":["dbl"],"align":["right"]},{"label":["log2FoldChange"],"name":[2],"type":["dbl"],"align":["right"]},{"label":["lfcSE"],"name":[3],"type":["dbl"],"align":["right"]},{"label":["stat"],"name":[4],"type":["dbl"],"align":["right"]},{"label":["pvalue"],"name":[5],"type":["dbl"],"align":["right"]},{"label":["padj"],"name":[6],"type":["dbl"],"align":["right"]},{"label":["external_gene_name"],"name":[7],"type":["chr"],"align":["left"]}],"data":[{"1":"6393.15468","2":"3.973568","3":"0.4443965","4":"8.941491","5":"3.839536e-19","6":"5.727052e-15","7":"CYP2C49","_rn_":"ENSSSCG00000010488"},{"1":"2801.19460","2":"2.613967","3":"0.3665907","4":"7.130480","5":"1.000192e-12","6":"3.904592e-09","7":"CYP2B22","_rn_":"ENSSSCG00000003006"},{"1":"19174.31905","2":"3.483967","3":"0.4890347","4":"7.124171","5":"1.047088e-12","6":"3.904592e-09","7":"","_rn_":"ENSSSCG00000035347"},{"1":"1808.13544","2":"1.367359","3":"0.1998482","4":"6.841987","5":"7.810245e-12","6":"2.329952e-08","7":"ACAD11","_rn_":"ENSSSCG00000011629"},{"1":"6196.04547","2":"2.091092","3":"0.3108987","4":"6.725960","5":"1.744389e-11","6":"4.336551e-08","7":"PCK2","_rn_":"ENSSSCG00000002009"},{"1":"856.82923","2":"1.413819","3":"0.2136912","4":"6.616179","5":"3.686019e-11","6":"6.872582e-08","7":"FAM131C","_rn_":"ENSSSCG00000003470"},{"1":"1948.81365","2":"1.807710","3":"0.2748258","4":"6.577659","5":"4.779136e-11","6":"7.920621e-08","7":"GPD1","_rn_":"ENSSSCG00000000214"},{"1":"2653.33191","2":"1.279816","3":"0.1997397","4":"6.407421","5":"1.480017e-10","6":"2.207594e-07","7":"DHRS4","_rn_":"ENSSSCG00000002013"},{"1":"7670.23915","2":"2.625450","3":"0.4177314","4":"6.285019","5":"3.278136e-10","6":"4.445153e-07","7":"PCK1","_rn_":"ENSSSCG00000007507"},{"1":"10044.81725","2":"1.569346","3":"0.2519773","4":"6.228127","5":"4.720450e-10","6":"5.867519e-07","7":"FBP1","_rn_":"ENSSSCG00000031174"},{"1":"786.81879","2":"1.190768","3":"0.1940909","4":"6.135107","5":"8.510170e-10","6":"9.764438e-07","7":"NHEJ1","_rn_":"ENSSSCG00000016205"},{"1":"127.14658","2":"2.008987","3":"0.3312230","4":"6.065360","5":"1.316585e-09","6":"1.309213e-06","7":"INMT","_rn_":"ENSSSCG00000016676"},{"1":"231.21993","2":"2.435851","3":"0.4039108","4":"6.030667","5":"1.632848e-09","6":"1.522223e-06","7":"","_rn_":"ENSSSCG00000024029"},{"1":"8982.67517","2":"1.503414","3":"0.2508662","4":"5.992891","5":"2.061430e-09","6":"1.808723e-06","7":"CYP2C42","_rn_":"ENSSSCG00000024537"},{"1":"2457.03676","2":"2.728921","3":"0.4671101","4":"5.842135","5":"5.153594e-09","6":"3.740091e-06","7":"PC","_rn_":"ENSSSCG00000012926"},{"1":"5173.72167","2":"1.601601","3":"0.2793495","4":"5.733324","5":"9.848101e-09","6":"6.677012e-06","7":"","_rn_":"ENSSSCG00000032419"},{"1":"612.40452","2":"1.584233","3":"0.2819470","4":"5.618905","5":"1.921718e-08","6":"1.194348e-05","7":"","_rn_":"ENSSSCG00000009213"},{"1":"65.71552","2":"3.149597","3":"0.5613114","4":"5.611141","5":"2.009970e-08","6":"1.199228e-05","7":"SLC39A2","_rn_":"ENSSSCG00000029754"},{"1":"717.44509","2":"1.160344","3":"0.2077160","4":"5.586204","5":"2.320862e-08","6":"1.317744e-05","7":"XK","_rn_":"ENSSSCG00000031868"},{"1":"6283.12989","2":"1.589666","3":"0.2848127","4":"5.581444","5":"2.385297e-08","6":"1.317744e-05","7":"","_rn_":"ENSSSCG00000007980"},{"1":"1348.94023","2":"1.542767","3":"0.2833749","4":"5.444261","5":"5.202091e-08","6":"2.771228e-05","7":"AMN","_rn_":"ENSSSCG00000002524"},{"1":"2794.71496","2":"1.005229","3":"0.1854653","4":"5.420036","5":"5.958700e-08","6":"3.031859e-05","7":"ANO10","_rn_":"ENSSSCG00000011296"},{"1":"4846.75329","2":"1.295719","3":"0.2392432","4":"5.415907","5":"6.097865e-08","6":"3.031859e-05","7":"AHCYL2","_rn_":"ENSSSCG00000016568"},{"1":"960.39581","2":"2.276539","3":"0.4217699","4":"5.397585","5":"6.754381e-08","6":"3.249947e-05","7":"DPEP1","_rn_":"ENSSSCG00000021971"},{"1":"14399.04661","2":"1.357831","3":"0.2529410","4":"5.368171","5":"7.953908e-08","6":"3.707515e-05","7":"PGM1","_rn_":"ENSSSCG00000003812"},{"1":"604.59476","2":"1.281913","3":"0.2394638","4":"5.353264","5":"8.638161e-08","6":"3.904449e-05","7":"ABHD1","_rn_":"ENSSSCG00000008554"},{"1":"6441.51880","2":"1.056326","3":"0.1990740","4":"5.306199","5":"1.119349e-07","6":"4.910649e-05","7":"ACADVL","_rn_":"ENSSSCG00000017947"},{"1":"33.64856","2":"2.733557","3":"0.5197096","4":"5.259779","5":"1.442289e-07","6":"5.814374e-05","7":"","_rn_":"ENSSSCG00000026798"},{"1":"354.23369","2":"1.039964","3":"0.1988622","4":"5.229574","5":"1.699009e-07","6":"6.498054e-05","7":"SPSB2","_rn_":"ENSSSCG00000025460"},{"1":"15055.24192","2":"1.159256","3":"0.2228527","4":"5.201892","5":"1.972703e-07","6":"7.176788e-05","7":"CAT","_rn_":"ENSSSCG00000013302"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>


I will use the list to annotate the total DE genes using the Biomart in Ensembl.And get the top 6 with lowest padjusted.


```r
#I will get the IDS and will use the following code
gene_names<- getBM(attributes = c("ensembl_gene_id","external_gene_name"), filters= "ensembl_gene_id", values = rownames(resSig_Salmon), mart = Pig_ensembl)
```

```
## Batch submitting query [==========================] 100% eta: 0s
```

```r
gene_names<-transform(merge(as.data.frame(resSig_Salmon),gene_names,by.x=0, by.y ="ensembl_gene_id",all=TRUE), row.names=Row.names, Row.names=NULL)
gene_names<-as.data.frame(gene_names[order(gene_names$padj),])
head(gene_names)
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":[""],"name":["_rn_"],"type":[""],"align":["left"]},{"label":["baseMean"],"name":[1],"type":["dbl"],"align":["right"]},{"label":["log2FoldChange"],"name":[2],"type":["dbl"],"align":["right"]},{"label":["lfcSE"],"name":[3],"type":["dbl"],"align":["right"]},{"label":["stat"],"name":[4],"type":["dbl"],"align":["right"]},{"label":["pvalue"],"name":[5],"type":["dbl"],"align":["right"]},{"label":["padj"],"name":[6],"type":["dbl"],"align":["right"]},{"label":["external_gene_name"],"name":[7],"type":["chr"],"align":["left"]}],"data":[{"1":"6393.1547","2":"3.973568","3":"0.4443965","4":"8.941491","5":"3.839536e-19","6":"5.727052e-15","7":"CYP2C49","_rn_":"ENSSSCG00000010488"},{"1":"2801.1946","2":"2.613967","3":"0.3665907","4":"7.130480","5":"1.000192e-12","6":"3.904592e-09","7":"CYP2B22","_rn_":"ENSSSCG00000003006"},{"1":"135.4037","2":"-2.460382","3":"0.3415207","4":"-7.204197","5":"5.838679e-13","6":"3.904592e-09","7":"","_rn_":"ENSSSCG00000032158"},{"1":"19174.3190","2":"3.483967","3":"0.4890347","4":"7.124171","5":"1.047088e-12","6":"3.904592e-09","7":"","_rn_":"ENSSSCG00000035347"},{"1":"1808.1354","2":"1.367359","3":"0.1998482","4":"6.841987","5":"7.810245e-12","6":"2.329952e-08","7":"ACAD11","_rn_":"ENSSSCG00000011629"},{"1":"6196.0455","2":"2.091092","3":"0.3108987","4":"6.725960","5":"1.744389e-11","6":"4.336551e-08","7":"PCK2","_rn_":"ENSSSCG00000002009"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

## Diagnostic plots

To plot the counts of specific genes we can use the name of the gene or a a specific value related to it.

### Volcano plot 


```r
# volcano plot 
plot(res$log2FoldChange, -log(res$pvalue),main ="Volcano plot Salmon data from DESeq2", xlab= "Log2FoldChange", ylab = "-log(Pvalue)", pch = 21, col=ifelse(res$pvalue <0.001,"red","black"), xlim= c(-5,5), ylim =c(0,25), abline(v=c(-2,2)))
```

![](C:/Users/rpmello/Desktop/Data Analysis RNASeq/Projeto_Salmon_DESeq_EdgeR/Output/FFD_vs_Goat_milkD_Salmon_R_with_outliers/FiguresVolcano_plot-1.png)<!-- -->

### Plotting the gene with lowest p. adjusted


```r
# using the lower padj
topGene<- row.names(resSig_Salmon[which.min(resSig_Salmon$padj),])
plotCounts(dds,gene= topGene,main = gene_names$external_gene_name[1], intgroup="Group")
```

![](C:/Users/rpmello/Desktop/Data Analysis RNASeq/Projeto_Salmon_DESeq_EdgeR/Output/FFD_vs_Goat_milkD_Salmon_R_with_outliers/Figureslowest_padj-1.png)<!-- -->

### to do the MA plot:


```r
plotMA(res, ylim=c(-20,20))
```

![](C:/Users/rpmello/Desktop/Data Analysis RNASeq/Projeto_Salmon_DESeq_EdgeR/Output/FFD_vs_Goat_milkD_Salmon_R_with_outliers/FiguresMA_plot-1.png)<!-- -->

### To plot dispersion.(within group variability of each gene )


```r
plotDispEsts(dds)
```

![](C:/Users/rpmello/Desktop/Data Analysis RNASeq/Projeto_Salmon_DESeq_EdgeR/Output/FFD_vs_Goat_milkD_Salmon_R_with_outliers/Figuresdispersion_plot-1.png)<!-- -->

### histogram of pvalues


```r
hist(res$pvalue[res$baseMean>1], breaks=20, col="grey50", border="white")
```

![](C:/Users/rpmello/Desktop/Data Analysis RNASeq/Projeto_Salmon_DESeq_EdgeR/Output/FFD_vs_Goat_milkD_Salmon_R_with_outliers/Figurespvalues_histogram-1.png)<!-- -->

### Clustering
Clustering the rlog counts of the 50 more variable DE genes


```r
library(genefilter)
#most hihgly variable genes
topVarGenes<- head(order(-rowVars(assay(rlogdds))),50)
colors<- colorRampPalette(rev(brewer.pal(9,"PuOr")))(255)
sidecols<- c("grey", "dodgerblue")[rlogdds$Group]
mat<- assay(rlogdds)[topVarGenes,]
mat<- mat - rowMeans(mat)
colnames(mat)<- paste0(rlogdds$Group)
heatmap.2(mat,Colv=F, trace="none", col=colors, ColSideColors=sidecols, labRow=F, mar=c(10,2),scale="row")
```

```
## Warning in heatmap.2(mat, Colv = F, trace = "none", col = colors,
## ColSideColors = sidecols, : Discrepancy: Colv is FALSE, while dendrogram is
## `both'. Omitting column dendogram.
```

![](C:/Users/rpmello/Desktop/Data Analysis RNASeq/Projeto_Salmon_DESeq_EdgeR/Output/FFD_vs_Goat_milkD_Salmon_R_with_outliers/FiguresClustering_rlog-1.png)<!-- -->


### Doing the clustering with normalized counts instead of rlog Counts

TPM of the 150 more variable DE genes(if DE genes < 150, all genes)




```r
# Prepare for the heatmap

# Get some nicer colours
mypalette <- brewer.pal(11,"RdYlBu")

morecols <- colorRampPalette(mypalette)

# Load library for heatmap
heatmap.2(highly_variable_norm_counts, col=rev(morecols(50)), trace="none",
              main="Most variable genes across samples", scale="row",
              labCol = labels)
```

![](C:/Users/rpmello/Desktop/Data Analysis RNASeq/Projeto_Salmon_DESeq_EdgeR/Output/FFD_vs_Goat_milkD_Salmon_R_with_outliers/Figurestpm_clustering-1.png)<!-- -->

```r
#To save the gene lists in both the cluster( 100 more variable genes in the resSig_Salmon2 table) and in the resSig_Salmon2 so I can used the transcripts to annotation and the other downstream application like panther or DAVID.
```

Cluster with all the DE genes using TPM



```r
DE_counts<- tpmDE
heatmap.2(DE_counts, col=rev(morecols(50)), trace="none",
              main="Most variable genes across samples", scale="row",
              labCol = labels)
```

![](C:/Users/rpmello/Desktop/Data Analysis RNASeq/Projeto_Salmon_DESeq_EdgeR/Output/FFD_vs_Goat_milkD_Salmon_R_with_outliers/Figurescluster_tpmde_all1-1.png)<!-- -->

# Pathway analysis using DAVID and Reactome


I will use the lists to do the Pathway analysis with ReactomePA and DAVIDWebService

Fist, geting the GO terms for the DEgenes (padj,0.1, L2FC 1)- this will show only in R. And for that I will connect to DAVID. 


To see the percentage of the genes that were identified by the DAVID


```r
DAVID_results_DESeq$inDavid
```

[1] 0.6107226

And then you can create the results charts from DAVID and filter it by biological process:


```r
DAVID_results_chart<- getFunctionalAnnotationChart(david)
#If I want to create a file with the chart information I would use the following code:
#DAVID_results_chart<-DAVIDFunctionalAnnotationChartFile(DAVID_results_chart)
DAVID_results_chart_DESeq_BP<-DAVID_results_chart[DAVID_results_chart$Category == "GOTERM_BP_DIRECT" & DAVID_results_chart$PValue < 0.05,]
```

Plot of the top pathways and the evidence found in the list of DE genes.


```r
if(dim(DAVID_results_chart_DESeq_BP)[1]>0){
plot2D(DAVIDFunctionalAnnotationChart(DAVID_results_chart_DESeq_BP[1:5,]),
       color=c("FALSE"="black", "TRUE"="green"))
# We can see the top 5 GO pathways for biological process, and note how the evidence is true in our data for each of the genes in the pathway( if it is present on our sample or not)
#We can also use the chart to do a godag plot
} else {
  print("No significant term found")
}
```

![](C:/Users/rpmello/Desktop/Data Analysis RNASeq/Projeto_Salmon_DESeq_EdgeR/Output/FFD_vs_Goat_milkD_Salmon_R_with_outliers/Figuresplot2D_BP-1.png)<!-- -->

```r
DAVIDGODag_DESeq<-DAVIDGODag(DAVIDFunctionalAnnotationChart(DAVID_results_chart_DESeq_BP), type ="BP",pvalueCutoff=.001)
#I can isolate the significant ones by doing a dataframe but the resulting plot is the same 
#sig_DAVID_RES_CHART<- DAVID_results_chart_BP[DAVID_results_chart_BP$PValue <0.001,]
#sig_DAVID_RES_GODAG<- DAVIDGODag(DAVIDFunctionalAnnotationChart(sig_DAVID_RES_CHART), type ="BP",pvalueCutoff=.001)
#we used a cutoff of 0.001 f or the pvalue. And now we can inspect the Dag object
```

The Pathways with pvalue lower than 0.0001 and table containing the pathways and pvalues.


```r
sigCategories(DAVIDGODag_DESeq, p=0.0001)
```

NULL

```r
summary(DAVIDGODag_DESeq)
```

```
## Warning: No results met the specified criteria. Returning 0-row data.frame
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["GOBPID"],"name":[1],"type":["chr"],"align":["left"]},{"label":["Pvalue"],"name":[2],"type":["dbl"],"align":["right"]},{"label":["Count"],"name":[3],"type":["int"],"align":["right"]},{"label":["Size"],"name":[4],"type":["int"],"align":["right"]},{"label":["Term"],"name":[5],"type":["chr"],"align":["left"]}],"data":[],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

```r
show(DAVIDGODag_DESeq)
```

Gene to GO BP  test for over-representation 
0 GO BP ids tested (0 have p < 0.001)
Selected gene set size: 0 

```
## Warning in max(popTotals(r)): no non-missing arguments to max; returning -
## Inf
```

    Gene universe size: -Inf 
    Annotation package: DAVID.website 

```r
# we can see by the FDR column that only the first 3 pathway are significant
```

GoDag term graph 



## Using GOplot to visualize the GOterm from DAVID


First, we have to put the data in the format required by the package.



Now, that we have the data let's try to plot it using GOplot.



### Plot the pathway colered by z-score significance

```r
if(dim(david_fun)[1]>0){
print(GOBar(circ, order.by.zscore = T ))}else {
  print("No significant terms")
}
```

![](C:/Users/rpmello/Desktop/Data Analysis RNASeq/Projeto_Salmon_DESeq_EdgeR/Output/FFD_vs_Goat_milkD_Salmon_R_with_outliers/FiguresGoBar-1.png)<!-- -->

###Bubble plot of pathways

```r
if(min(circ$adj_pval)<0.05){
print(GOBubble(circ, labels = 1))} else {
  print("No significant terms")
}
```

[1] "No significant terms"

### Plot significant enriched pathways and DE Genes

This plot is interesting because it shows which genes are up and down on the pathway

```r
if(dim(david_fun)[1]>0){
nsub<- ifelse(dim(circ[!duplicated(circ$term), ])[1] > 10, 10, dim(circ[!duplicated(circ$term), ])[1]); 
print(GOCircle(circ,nsub = nsub))} else {
  print("No significant terms")
}
```

![](C:/Users/rpmello/Desktop/Data Analysis RNASeq/Projeto_Salmon_DESeq_EdgeR/Output/FFD_vs_Goat_milkD_Salmon_R_with_outliers/FiguresGOCircle-1.png)<!-- -->TableGrob (1 x 2) "arrange": 2 grobs
  z     cells    name               grob
1 1 (1-1,1-1) arrange     gtable[layout]
2 2 (1-1,2-2) arrange gtable[colhead-fg]

I can also plot the relationship between the DE genes and the most enriched pathways


```r
if(dim(david_fun)[1]>0){
subData <- subset(circ, term %in% process);
genes<-subset(genelist, ID %in% subData$genes);
All_genes<- getBM(attributes = c("ensembl_gene_id","external_gene_name"), filters= "ensembl_gene_id", values = row.names(res), mart = Pig_ensembl);
All_genes$external_gene_name<-ifelse(All_genes$external_gene_name=="", All_genes$ensembl_gene_id, All_genes$external_gene_name)
names_subData<- subset(All_genes, ensembl_gene_id %in% subData$genes);
names_subData$external_gene_name<- ifelse(names_subData$external_gene_name == "", names_subData$ensembl_gene_id, names_subData$external_gene_name);
chord <- chord_dat(circ, genes = genes, process = dataGOplot$process);
chord_names<-as.data.frame(cbind(chord, names_subData[,"external_gene_name"][match(rownames(chord), names_subData$ensembl_gene_id)]));
row.names(chord)<-chord_names$V7;
chord<-chord[,colSums(chord != 0) > 0];
print(GOChord(chord));
terms<-dataGOplot$process[!is.na(dataGOplot$process)]
if(length(terms)>2){
print(GOCluster(circ, terms, clust.by = "term", lfc.col = c('darkgoldenrod1','black','cyan1')))}} else {
  print("No significant term found!")
}
```

```
## Batch submitting query [=>------------------------] 6% eta: 6s Batch
## submitting query [=>------------------------] 9% eta: 7s Batch submitting
## query [==>-----------------------] 12% eta: 7s Batch submitting
## query [===>----------------------] 16% eta: 7s Batch submitting
## query [====>---------------------] 19% eta: 6s Batch submitting
## query [=====>--------------------] 22% eta: 6s Batch submitting
## query [=====>--------------------] 25% eta: 6s Batch submitting
## query [======>-------------------] 28% eta: 7s Batch submitting
## query [=======>------------------] 31% eta: 9s Batch submitting
## query [========>-----------------] 34% eta: 10s Batch submitting
## query [=========>----------------] 38% eta: 19s Batch submitting
## query [==========>---------------] 41% eta: 17s Batch submitting
## query [==========>---------------] 44% eta: 16s Batch submitting
## query [===========>--------------] 47% eta: 14s Batch submitting
## query [============>-------------] 50% eta: 13s Batch submitting
## query [=============>------------] 53% eta: 12s Batch submitting
## query [==============>-----------] 56% eta: 11s Batch submitting
## query [==============>-----------] 59% eta: 10s Batch submitting
## query [===============>----------] 62% eta: 9s Batch submitting
## query [================>---------] 66% eta: 8s Batch submitting
## query [=================>--------] 69% eta: 7s Batch submitting
## query [==================>-------] 72% eta: 7s Batch submitting
## query [===================>------] 75% eta: 6s Batch submitting
## query [===================>------] 78% eta: 7s Batch submitting
## query [====================>-----] 81% eta: 6s Batch submitting
## query [=====================>----] 84% eta: 5s Batch submitting
## query [======================>---] 88% eta: 4s Batch submitting
## query [=======================>--] 91% eta: 3s Batch submitting
## query [=======================>--] 94% eta: 2s Batch submitting
## query [========================>-] 97% eta: 1s Batch submitting query
## [==========================] 100% eta: 0s
```

```
## Warning: Using size for a discrete variable is not advised.
```

```
## Warning: Removed 5 rows containing missing values (geom_point).
```

![](C:/Users/rpmello/Desktop/Data Analysis RNASeq/Projeto_Salmon_DESeq_EdgeR/Output/FFD_vs_Goat_milkD_Salmon_R_with_outliers/FiguresGOplot_terms_and_genes-1.png)

```
## Warning: Using size for a discrete variable is not advised.

## Warning: Removed 5 rows containing missing values (geom_point).
```

![](C:/Users/rpmello/Desktop/Data Analysis RNASeq/Projeto_Salmon_DESeq_EdgeR/Output/FFD_vs_Goat_milkD_Salmon_R_with_outliers/FiguresGOplot_terms_and_genes-2.png)

```r
#the GOCluster indicated in the outer ring the pathway that the gene is associated to, the inner ring shows the log2fc of the gene and the clustering the gene by category, euclidean average 
```

## Using Biomart to convert the genes names into entrez ID (human) so I can use ReactomePA to do the pathway analysis

First table converting all the genes to entrex id and then filtering by padj < 0.1
 


## Using reactome to analize subsetted from all genes




```r
if(!is.null(x)){
if(min(x@result$p.adjust)<0.05){
sprintf("Barplot for the most enriched reactome pathway for Salmon/DESeq analysis for comparison %s vc %s", groups[1],groups[2]);
print(barplot(x, showCategory = 30));
sprintf("Dotplot for the most enriched reactome pathway for Salmon/DESeq analysis for comparison %s vc %s", groups[1],groups[2]);
print(dotplot(x, showCategory=15));
sprintf("Enrichment map plot for the most enriched reactome pathway for Salmon/DESeq analysis for comparison %s vc %s", groups[1],groups[2]);
print(emapplot(x, showCategory = 15, color = "p.adjust",layout = "kk",colorEdge = TRUE)); 
print(cnetplot(x,showCategory = 5, categorySize = "pvalue", foldChange = DE_genes_entrez_rank$log2FoldChange, node_label= F, colorEdge = T))}} else {
  print("no enriched term found")
}
```

```
## wrong orderBy parameter; set to default `orderBy = "x"`
```

![](C:/Users/rpmello/Desktop/Data Analysis RNASeq/Projeto_Salmon_DESeq_EdgeR/Output/FFD_vs_Goat_milkD_Salmon_R_with_outliers/FiguresReactome_plots-1.png)![](C:/Users/rpmello/Desktop/Data Analysis RNASeq/Projeto_Salmon_DESeq_EdgeR/Output/FFD_vs_Goat_milkD_Salmon_R_with_outliers/FiguresReactome_plots-2.png)![](C:/Users/rpmello/Desktop/Data Analysis RNASeq/Projeto_Salmon_DESeq_EdgeR/Output/FFD_vs_Goat_milkD_Salmon_R_with_outliers/FiguresReactome_plots-3.png)![](C:/Users/rpmello/Desktop/Data Analysis RNASeq/Projeto_Salmon_DESeq_EdgeR/Output/FFD_vs_Goat_milkD_Salmon_R_with_outliers/FiguresReactome_plots-4.png)

```r
y<- gsePathway(DE_genes_list, nPerm = 1000,minGSSize = 10, pvalueCutoff = 0.05, pAdjustMethod = "BH")
```

```
## preparing geneSet collections...
```

```
## GSEA analysis...
```

```
## Warning in fgsea(pathways = geneSets, stats = geneList, nperm = nPerm,
## minSize = minGSSize, : There are duplicate gene names, fgsea may produce
## unexpected results
```

```
## leading edge analysis...
```

```
## done...
```

```r
if(nrow(y) > 0){
print(gseaplot(y, geneSetID =(as.data.frame(y))[1,1],title = as.character((as.data.frame(y))[1,2])));
print(cnetplot(y, categorySize = "pvalue", foldChange = DE_genes_entrez_rank$log2FoldChange,colorEdge = T));
print(emapplot(y, showCategory = 15, color = "p.adjust",layout = "kk"));
}
```

![](C:/Users/rpmello/Desktop/Data Analysis RNASeq/Projeto_Salmon_DESeq_EdgeR/Output/FFD_vs_Goat_milkD_Salmon_R_with_outliers/FiguresReactome_plots-5.png)![](C:/Users/rpmello/Desktop/Data Analysis RNASeq/Projeto_Salmon_DESeq_EdgeR/Output/FFD_vs_Goat_milkD_Salmon_R_with_outliers/FiguresReactome_plots-6.png)![](C:/Users/rpmello/Desktop/Data Analysis RNASeq/Projeto_Salmon_DESeq_EdgeR/Output/FFD_vs_Goat_milkD_Salmon_R_with_outliers/FiguresReactome_plots-7.png)

```r
#DEunique<- DE_genes_list[!duplicated(names(DE_genes_list))]
```

## Reactome of the upregulated genes




```r
if(!is.null(x)){
if(min(x@result$p.adjust)<0.05){
sprintf("Barplot for the most enriched reactome pathway for Salmon/DESeq analysis for comparison %s vc %s up regulated genes", groups[1],groups[2]);
print(barplot(x, showCategory = 30));
sprintf("Dotplot for the most enriched reactome pathway for Salmon/DESeq analysis for comparison %s vc %s up regulated genes", groups[1],groups[2]);
print(dotplot(x, showCategory=15));
sprintf("Enrichment map plot for the most enriched reactome pathway for Salmon/DESeq analysis for comparison %s vc %s upregulated genes", groups[1],groups[2]);
print(emapplot(x, showCategory = 15, color = "p.adjust",layout = "kk",colorEdge = TRUE));
print(cnetplot(x,showCategory = 5, categorySize = "pvalue", foldChange = up_genes_list, node_label= F, colorEdge = T));
}}
```

```
## wrong orderBy parameter; set to default `orderBy = "x"`
```

![](C:/Users/rpmello/Desktop/Data Analysis RNASeq/Projeto_Salmon_DESeq_EdgeR/Output/FFD_vs_Goat_milkD_Salmon_R_with_outliers/Figuresreactome_plots_up-1.png)![](C:/Users/rpmello/Desktop/Data Analysis RNASeq/Projeto_Salmon_DESeq_EdgeR/Output/FFD_vs_Goat_milkD_Salmon_R_with_outliers/Figuresreactome_plots_up-2.png)![](C:/Users/rpmello/Desktop/Data Analysis RNASeq/Projeto_Salmon_DESeq_EdgeR/Output/FFD_vs_Goat_milkD_Salmon_R_with_outliers/Figuresreactome_plots_up-3.png)![](C:/Users/rpmello/Desktop/Data Analysis RNASeq/Projeto_Salmon_DESeq_EdgeR/Output/FFD_vs_Goat_milkD_Salmon_R_with_outliers/Figuresreactome_plots_up-4.png)

```r
y<- gsePathway(up_genes_list, nPerm = 1000,minGSSize = 10, pvalueCutoff = 0.05, pAdjustMethod = "BH")
```

```
## preparing geneSet collections...
```

```
## GSEA analysis...
```

```
## no term enriched under specific pvalueCutoff...
```

```r
if(nrow(y) > 0){
print(gseaplot(y, geneSetID =(as.data.frame(y))[1,1],title = as.character((as.data.frame(y))[1,2])));
print(cnetplot(y, categorySize = "pvalue", foldChange = upentrez$log2FoldChange,colorEdge = T));
print(emapplot(y, showCategory = 15, color = "p.adjust",layout = "kk"));
}
```

## Reactome of downregulated genes




```r
if(!is.null(x)){
if(min(x@result$p.adjust)<0.05){
sprintf("Barplot for the most enriched reactome pathway for Salmon/DESeq analysis for comparison %s vc %s downregulated genes", groups[1],groups[2]);
print(barplot(x, showCategory = 30));
sprintf("Dotplot for the most enriched reactome pathway for Salmon/DESeq analysis for comparison %s vc %s downregulated genes", groups[1],groups[2]);
print(dotplot(x, showCategory = 15));
sprintf("Enrichment map plot for the most enriched reactome pathway for Salmon/DESeq analysis for comparison %s vc %s downregulated genes", groups[1],groups[2])
print(emapplot(x, showCategory = 15, color = "p.adjust",layout = "kk")); 
print(cnetplot(x,showCategory = 5, categorySize = "pvalue", foldChange = down_genes_list, node_label= FALSE, colorEdge = TRUE));
}}
```

```
## wrong orderBy parameter; set to default `orderBy = "x"`
```

![](C:/Users/rpmello/Desktop/Data Analysis RNASeq/Projeto_Salmon_DESeq_EdgeR/Output/FFD_vs_Goat_milkD_Salmon_R_with_outliers/Figuresreactomrdown_plots-1.png)![](C:/Users/rpmello/Desktop/Data Analysis RNASeq/Projeto_Salmon_DESeq_EdgeR/Output/FFD_vs_Goat_milkD_Salmon_R_with_outliers/Figuresreactomrdown_plots-2.png)![](C:/Users/rpmello/Desktop/Data Analysis RNASeq/Projeto_Salmon_DESeq_EdgeR/Output/FFD_vs_Goat_milkD_Salmon_R_with_outliers/Figuresreactomrdown_plots-3.png)![](C:/Users/rpmello/Desktop/Data Analysis RNASeq/Projeto_Salmon_DESeq_EdgeR/Output/FFD_vs_Goat_milkD_Salmon_R_with_outliers/Figuresreactomrdown_plots-4.png)

```r
y<- gsePathway(down_genes_list, nPerm = 1000,minGSSize = 10, pvalueCutoff = 0.05, pAdjustMethod = "BH")
```

```
## preparing geneSet collections...
```

```
## GSEA analysis...
```

```
## Warning in fgsea(pathways = geneSets, stats = geneList, nperm = nPerm,
## minSize = minGSSize, : There are duplicate gene names, fgsea may produce
## unexpected results
```

```
## leading edge analysis...
```

```
## done...
```

```r
if(nrow(y) > 0){
print(gseaplot(y, geneSetID =(as.data.frame(y))[1,1],title = as.character((as.data.frame(y))[1,2])));
print(cnetplot(y, categorySize = "pvalue", foldChange = downentrez$log2FoldChange,colorEdge = T));
print(emapplot(y, showCategory = 15, color = "p.adjust",layout = "kk"))
}
```

![](C:/Users/rpmello/Desktop/Data Analysis RNASeq/Projeto_Salmon_DESeq_EdgeR/Output/FFD_vs_Goat_milkD_Salmon_R_with_outliers/Figuresreactomrdown_plots-5.png)![](C:/Users/rpmello/Desktop/Data Analysis RNASeq/Projeto_Salmon_DESeq_EdgeR/Output/FFD_vs_Goat_milkD_Salmon_R_with_outliers/Figuresreactomrdown_plots-6.png)![](C:/Users/rpmello/Desktop/Data Analysis RNASeq/Projeto_Salmon_DESeq_EdgeR/Output/FFD_vs_Goat_milkD_Salmon_R_with_outliers/Figuresreactomrdown_plots-7.png)

```r
#downunique<- down_genes_list[!duplicated(names(down_genes_list))]
#viewPathway(as.character((as.data.frame(y))[1,2]), readable = T, foldChange = downunique )
```


# EdgeR analysis

We can repeat the same analysis using edgeR as the inference engine (Robinson, McCarthy, and Smyth 2009). The following code incorporates the average transcript length matrix as an offset for an edgeR analysis.

But I will use the tximport object to get the data to EdgeR, and normalize the data by the length of the transcript.



## The results from the EdgeR

### Counts of the EdgeRdataset


```r
head(as.data.frame(EdgeR_s$counts))
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":[""],"name":["_rn_"],"type":[""],"align":["left"]},{"label":["FF1"],"name":[1],"type":["dbl"],"align":["right"]},{"label":["FF2"],"name":[2],"type":["dbl"],"align":["right"]},{"label":["FF3"],"name":[3],"type":["dbl"],"align":["right"]},{"label":["FF4"],"name":[4],"type":["dbl"],"align":["right"]},{"label":["Goat_milk1"],"name":[5],"type":["dbl"],"align":["right"]},{"label":["Goat_milk2"],"name":[6],"type":["dbl"],"align":["right"]},{"label":["Goat_milk3"],"name":[7],"type":["dbl"],"align":["right"]},{"label":["Goat_milk4"],"name":[8],"type":["dbl"],"align":["right"]}],"data":[{"1":"915.0468","2":"492.0342","3":"583.0046","4":"359.0002","5":"412.0425","6":"504.0004","7":"186.000","8":"413.0003","_rn_":"ENSSSCG00000000002"},{"1":"1285.9465","2":"2501.8286","3":"4098.0959","4":"3051.9850","5":"5050.5516","6":"3578.3783","7":"3018.806","8":"5359.4435","_rn_":"ENSSSCG00000000003"},{"1":"144.0000","2":"76.0000","3":"155.0000","4":"69.0000","5":"135.0000","6":"174.0000","7":"69.000","8":"219.0000","_rn_":"ENSSSCG00000000005"},{"1":"63.0000","2":"429.0000","3":"786.0000","4":"772.0000","5":"769.0000","6":"209.0000","7":"518.000","8":"535.0000","_rn_":"ENSSSCG00000000006"},{"1":"300.0000","2":"336.9997","3":"479.0001","4":"386.0000","5":"515.0004","6":"475.0000","7":"281.000","8":"434.9999","_rn_":"ENSSSCG00000000007"},{"1":"752.7090","2":"2926.3080","3":"1617.8660","4":"2794.2020","5":"1858.9030","6":"3818.9100","7":"1629.245","8":"2532.3820","_rn_":"ENSSSCG00000000010"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

### MDS plot 


```r
plotMDS(EdgeR_s)
```

![](C:/Users/rpmello/Desktop/Data Analysis RNASeq/Projeto_Salmon_DESeq_EdgeR/Output/FFD_vs_Goat_milkD_Salmon_R_with_outliers/FiguresMDS_plot_EdgeR-1.png)<!-- -->

### DE analysis using exact test

I will use the exact test to do the pairwise comparison without any batch effect.

And the number of genes with pvalue < 0.1


```r
EdgeR_s$samples$W1<-set_n$W_1
design<-model.matrix(as.formula(~ W1+ group),data =EdgeR_s$samples)
DE_fit<-glmQLFit(EdgeR_s,design=design)
DE_single_factor_edgeR<- glmQLFTest(DE_fit)
sig_DE_EdgeR<-DE_single_factor_edgeR$table[DE_single_factor_edgeR$table$PValue < 0.1,]
dim(sig_DE_EdgeR)
```

[1] 5449    4

```r
#FDR_edgeR<-p.adjust(DE_single_factor_edgeR$table$PValue, method = "fdr")
#DE_Table<-cbind(DE_single_factor_edgeR$table,FDR_edgeR)
#sig_Table<- DE_Table[DE_Table$FDR_edgeR< 0.1 & DE_Table$logFC > 1 | DE_Table$logFC < -1,]
```

### The toptags table for the sig. DE genes

Number of Genes with log2FoldChange < -1 or > 1, and p.adjusted < 0.1 and the table with the values.


```r
Topgenes_sfactor_edgeR<-topTags(DE_single_factor_edgeR, n = nrow(sig_DE_EdgeR))
dim(Topgenes_sfactor_edgeR)
```

[1] 5449    5

```r
#The FDR is adj. Pvalue  
Topgenes_sfactor_edgeR_0.1_1<- 
Topgenes_sfactor_edgeR[Topgenes_sfactor_edgeR$table$FDR < 0.1   & Topgenes_sfactor_edgeR$table$logFC < -1 | Topgenes_sfactor_edgeR$table$logFC > 1,]
dim(Topgenes_sfactor_edgeR_0.1_1)
```

[1] 378   5

```r
head(Topgenes_sfactor_edgeR_0.1_1$table)
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":[""],"name":["_rn_"],"type":[""],"align":["left"]},{"label":["logFC"],"name":[1],"type":["dbl"],"align":["right"]},{"label":["logCPM"],"name":[2],"type":["dbl"],"align":["right"]},{"label":["F"],"name":[3],"type":["dbl"],"align":["right"]},{"label":["PValue"],"name":[4],"type":["dbl"],"align":["right"]},{"label":["FDR"],"name":[5],"type":["dbl"],"align":["right"]}],"data":[{"1":"-1.896009","2":"0.9652045","3":"63.21458","4":"2.028142e-05","5":"0.0815868","_rn_":"ENSSSCG00000008124"},{"1":"1.360668","2":"5.7280263","3":"62.57142","4":"2.114318e-05","5":"0.0815868","_rn_":"ENSSSCG00000011629"},{"1":"1.404454","2":"4.6584181","3":"58.33228","4":"2.809041e-05","5":"0.0815868","_rn_":"ENSSSCG00000003470"},{"1":"-2.457504","2":"2.0485676","3":"57.33096","4":"3.011974e-05","5":"0.0815868","_rn_":"ENSSSCG00000032158"},{"1":"3.960363","2":"7.5708887","3":"55.91165","4":"3.331135e-05","5":"0.0815868","_rn_":"ENSSSCG00000010488"},{"1":"1.271798","2":"6.3298862","3":"53.58803","4":"3.947646e-05","5":"0.0815868","_rn_":"ENSSSCG00000002013"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

```r
Top_Genes_Table<-Topgenes_sfactor_edgeR_0.1_1$table[order(Topgenes_sfactor_edgeR_0.1_1$table$FDR),]
saveRDS(Top_Genes_Table, file = file.path(output_dir,sprintf("%s_vs_%s_EdgeR_sig_Sal_w_outliers.RDS",groups[1], groups[2])))
```



## Get the down regulated genes 


```r
Topgenes_sfactor_edgeR_0.1_1_down<- Topgenes_sfactor_edgeR_0.1_1[Topgenes_sfactor_edgeR_0.1_1$table$logFC < 0, ]
Topgenes_sfactor_edgeR_0.1_1_down<- Topgenes_sfactor_edgeR_0.1_1_down[order(Topgenes_sfactor_edgeR_0.1_1_down$table$logFC), ]
head(Topgenes_sfactor_edgeR_0.1_1_down$table)
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":[""],"name":["_rn_"],"type":[""],"align":["left"]},{"label":["logFC"],"name":[1],"type":["dbl"],"align":["right"]},{"label":["logCPM"],"name":[2],"type":["dbl"],"align":["right"]},{"label":["F"],"name":[3],"type":["dbl"],"align":["right"]},{"label":["PValue"],"name":[4],"type":["dbl"],"align":["right"]},{"label":["FDR"],"name":[5],"type":["dbl"],"align":["right"]}],"data":[{"1":"-2.457504","2":"2.0485676","3":"57.33096","4":"3.011974e-05","5":"0.0815868","_rn_":"ENSSSCG00000032158"},{"1":"-1.896009","2":"0.9652045","3":"63.21458","4":"2.028142e-05","5":"0.0815868","_rn_":"ENSSSCG00000008124"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

```r
dim(Topgenes_sfactor_edgeR_0.1_1_down$table)
```

[1] 2 5

## Getting the upregulated genes


```r
Topgenes_sfactor_edgeR_0.1_1_up<- Topgenes_sfactor_edgeR_0.1_1[Topgenes_sfactor_edgeR_0.1_1$table$logFC > 0, ]
Topgenes_sfactor_edgeR_0.1_1_up<-Topgenes_sfactor_edgeR_0.1_1_up[order(-Topgenes_sfactor_edgeR_0.1_1_up$table$logFC), ]
head(Topgenes_sfactor_edgeR_0.1_1_up$table)
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":[""],"name":["_rn_"],"type":[""],"align":["left"]},{"label":["logFC"],"name":[1],"type":["dbl"],"align":["right"]},{"label":["logCPM"],"name":[2],"type":["dbl"],"align":["right"]},{"label":["F"],"name":[3],"type":["dbl"],"align":["right"]},{"label":["PValue"],"name":[4],"type":["dbl"],"align":["right"]},{"label":["FDR"],"name":[5],"type":["dbl"],"align":["right"]}],"data":[{"1":"3.960363","2":"7.570889","3":"55.911650","4":"3.331135e-05","5":"0.0815868","_rn_":"ENSSSCG00000010488"},{"1":"3.475221","2":"9.046668","3":"33.964591","4":"2.290356e-04","5":"0.1154857","_rn_":"ENSSSCG00000035347"},{"1":"3.061634","2":"4.604172","3":"16.066585","4":"2.930229e-03","5":"0.1160364","_rn_":"ENSSSCG00000007436"},{"1":"2.963120","2":"3.211625","3":"15.313370","4":"3.391273e-03","5":"0.1160364","_rn_":"ENSSSCG00000029592"},{"1":"2.835534","2":"2.694254","3":"13.111988","4":"5.352764e-03","5":"0.1185354","_rn_":"ENSSSCG00000039476"},{"1":"2.718650","2":"5.353619","3":"6.063944","4":"3.543552e-02","5":"0.1665550","_rn_":"ENSSSCG00000030269"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

```r
dim(Topgenes_sfactor_edgeR_0.1_1_up$table)
```

[1] 376   5

## Creating a volcano plot and a heatmap


```r
plot(DE_single_factor_edgeR$table$logFC,-log(DE_single_factor_edgeR$table$PValue),main ="Volcano plot", xlab= "Log2FoldChange", ylab = "-log(Pvalue)", pch = 21, col=ifelse(DE_single_factor_edgeR$table$PValue<0.001,"red","black"), xlim= c(-5,5), ylim =c(0,25), abline(v=c(-2,2)))
```

![](C:/Users/rpmello/Desktop/Data Analysis RNASeq/Projeto_Salmon_DESeq_EdgeR/Output/FFD_vs_Goat_milkD_Salmon_R_with_outliers/Figuresvolcano_plot_DE_EdgeR_Salmon-1.png)<!-- -->

heatmap using the top50 variable DE genes (normalized by library size)


```r
#Using normalized counts
counts_salmon_n <- cpm(EdgeR_s, normalized.lib.sizes = T)
Sig_counts<-counts_salmon_n[row.names(counts_salmon_n) %in% row.names(Topgenes_sfactor_edgeR_0.1_1$table),]
var_genes <- apply(Sig_counts, 1, var)
head(var_genes)
```

ENSSSCG00000000033 ENSSSCG00000000038 ENSSSCG00000000133 
         2097.7474          1396.1718         13796.8787 
ENSSSCG00000000206 ENSSSCG00000000214 ENSSSCG00000000252 
          259.6640           399.8063       1655370.2744 

```r
Sig_counts<- cbind(Sig_counts, var_genes)
Sig_counts<-Sig_counts[order(Sig_counts[,7],decreasing =T),]
len<-ifelse(dim(Sig_counts)[1]<50, dim(Sig_counts)[1],50)
heatmap.2(Sig_counts[1:len,1:(ncol(Sig_counts)-1)],  trace="none", 
              main="Most variable genes across samples", scale="row",col = rev(morecols(50)),
              labCol = labels )
```

![](C:/Users/rpmello/Desktop/Data Analysis RNASeq/Projeto_Salmon_DESeq_EdgeR/Output/FFD_vs_Goat_milkD_Salmon_R_with_outliers/Figuresnormalized_counts_heatmap_edger-1.png)<!-- -->

## Functional analysis of the EdgeR results

Using the DAVIDWEBServices to analize the DE genes and doing the plot2D of terms and evidence: 


```r
if(is.connected(david)== FALSE) {
  connect(david)
}
DAVID_results_EdgeR<- addList(david, row.names(Topgenes_sfactor_edgeR_0.1_1$table), idType = "ENSEMBL_GENE_ID" , listName = sprintf("%s_v_%s_Sig_genes_EdgeR",groups[1],groups[2]) ,listType = "Gene" )
# to see the p% of genes that were recognize on DAVID database
DAVID_results_EdgeR$inDavid
```

[1] 0.6402116

```r
DAVID_results_chart<- getFunctionalAnnotationChart(david)
DAVID_results_chart_EdgeR_BP<-DAVID_results_chart[DAVID_results_chart$Category == "GOTERM_BP_DIRECT" & DAVID_results_chart$PValue < 0.05,]
if(dim(DAVID_results_chart_EdgeR_BP)[1]>0){
plot2D(DAVIDFunctionalAnnotationChart(DAVID_results_chart_EdgeR_BP[1:5,]),
       color=c("FALSE"="black", "TRUE"="green"))
}else{
  print("No significant term found")
}
```

![](C:/Users/rpmello/Desktop/Data Analysis RNASeq/Projeto_Salmon_DESeq_EdgeR/Output/FFD_vs_Goat_milkD_Salmon_R_with_outliers/FiguresEdgeR_DAVID-1.png)<!-- -->

# Comparing the results from DESeq and EdgeR


```r
if(dim(DAVID_results_chart_DESeq_BP)[1]>0 & dim(DAVID_results_chart_EdgeR_BP)[1]>0){
sprintf("DESeq2_%s_%s", groups[1],groups[2])
head(DAVID_results_chart_DESeq_BP[order(DAVID_results_chart_DESeq_BP$PValue),],10)
sprintf("EdgeR_%s_%s", groups[1],groups[2])
head(DAVID_results_chart_EdgeR_BP[order(DAVID_results_chart_EdgeR_BP$PValue),],10)
}else{
  print("Either DESeq or EdgeR functional chart is empty.")
}
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":[""],"name":["_rn_"],"type":[""],"align":["left"]},{"label":["Category"],"name":[1],"type":["fctr"],"align":["left"]},{"label":["Term"],"name":[2],"type":["chr"],"align":["left"]},{"label":["Count"],"name":[3],"type":["int"],"align":["right"]},{"label":["X."],"name":[4],"type":["dbl"],"align":["right"]},{"label":["PValue"],"name":[5],"type":["dbl"],"align":["right"]},{"label":["Genes"],"name":[6],"type":["chr"],"align":["left"]},{"label":["List.Total"],"name":[7],"type":["int"],"align":["right"]},{"label":["Pop.Hits"],"name":[8],"type":["int"],"align":["right"]},{"label":["Pop.Total"],"name":[9],"type":["int"],"align":["right"]},{"label":["Fold.Enrichment"],"name":[10],"type":["dbl"],"align":["right"]},{"label":["Bonferroni"],"name":[11],"type":["dbl"],"align":["right"]},{"label":["Benjamini"],"name":[12],"type":["dbl"],"align":["right"]},{"label":["FDR"],"name":[13],"type":["dbl"],"align":["right"]}],"data":[{"1":"GOTERM_BP_DIRECT","2":"GO:0006094~gluconeogenesis","3":"4","4":"1.6528926","5":"0.001979985","6":"ENSSSCG00000000214, ENSSSCG00000002009, ENSSSCG00000007507, ENSSSCG00000012926","7":"173","8":"17","9":"11368","10":"15.461408","11":"0.7344423","12":"0.7344423","13":"2.927081","_rn_":"24"},{"1":"GOTERM_BP_DIRECT","2":"GO:0007156~homophilic cell adhesion via plasma membrane adhesion molecules","3":"6","4":"2.4793388","5":"0.004623997","6":"ENSSSCG00000014055, ENSSSCG00000006106, ENSSSCG00000003090, ENSSSCG00000006415, ENSSSCG00000017244, ENSSSCG00000022289","7":"173","8":"72","9":"11368","10":"5.475915","11":"0.9549791","12":"0.7878187","13":"6.711242","_rn_":"33"},{"1":"GOTERM_BP_DIRECT","2":"GO:0006730~one-carbon metabolic process","3":"4","4":"1.6528926","5":"0.004824872","6":"ENSSSCG00000016568, ENSSSCG00000005315, ENSSSCG00000006140, ENSSSCG00000007278","7":"173","8":"23","9":"11368","10":"11.427997","11":"0.9606654","12":"0.6599119","13":"6.993037","_rn_":"38"},{"1":"GOTERM_BP_DIRECT","2":"GO:0006879~cellular iron ion homeostasis","3":"4","4":"1.6528926","5":"0.006852441","6":"ENSSSCG00000016210, ENSSSCG00000012363, ENSSSCG00000011640, ENSSSCG00000023807","7":"173","8":"26","9":"11368","10":"10.109382","11":"0.9899488","12":"0.6833681","13":"9.793246","_rn_":"45"},{"1":"GOTERM_BP_DIRECT","2":"GO:0008203~cholesterol metabolic process","3":"4","4":"1.6528926","5":"0.010238923","6":"ENSSSCG00000003018, ENSSSCG00000003707, ENSSSCG00000004957, ENSSSCG00000013302","7":"173","8":"30","9":"11368","10":"8.761464","11":"0.9989771","12":"0.7476731","13":"14.295426","_rn_":"53"},{"1":"GOTERM_BP_DIRECT","2":"GO:0001501~skeletal system development","3":"4","4":"1.6528926","5":"0.023830890","6":"ENSSSCG00000017557, ENSSSCG00000007436, ENSSSCG00000016204, ENSSSCG00000006477","7":"173","8":"41","9":"11368","10":"6.410828","11":"0.9999999","12":"0.9320727","13":"30.339045","_rn_":"70"},{"1":"GOTERM_BP_DIRECT","2":"GO:0046718~viral entry into host cell","3":"3","4":"1.2396694","5":"0.026668851","6":"ENSSSCG00000003090, ENSSSCG00000003707, ENSSSCG00000005908","7":"173","8":"17","9":"11368","10":"11.596056","11":"1.0000000","12":"0.9244819","13":"33.313711","_rn_":"75"},{"1":"GOTERM_BP_DIRECT","2":"GO:0043171~peptide catabolic process","3":"3","4":"1.2396694","5":"0.029708740","6":"ENSSSCG00000004869, ENSSSCG00000021241, ENSSSCG00000023325","7":"173","8":"18","9":"11368","10":"10.951830","11":"1.0000000","12":"0.9197048","13":"36.368269","_rn_":"87"},{"1":"GOTERM_BP_DIRECT","2":"GO:0098869~cellular oxidant detoxification","3":"3","4":"1.2396694","5":"0.029708740","6":"ENSSSCG00000022742, ENSSSCG00000022351, ENSSSCG00000013302","7":"173","8":"18","9":"11368","10":"10.951830","11":"1.0000000","12":"0.9197048","13":"36.368269","_rn_":"88"},{"1":"GOTERM_BP_DIRECT","2":"GO:0044117~growth of symbiont in host","3":"2","4":"0.8264463","5":"0.030032768","6":"ENSSSCG00000027322, ENSSSCG00000025560","7":"173","8":"2","9":"11368","10":"65.710983","11":"1.0000000","12":"0.8963407","13":"36.686043","_rn_":"90"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

```r
# We can see the top 5 GO pathways for biological process, and note how the evidence is true in our data for each of the genes in the pathway( if it is present on our sample or not)
#We can also use the chart to do a godag plot
```

## Significant terms


```r
DAVIDGODag_EdgeR<-DAVIDGODag(DAVIDFunctionalAnnotationChart(DAVID_results_chart_EdgeR_BP), type ="BP",pvalueCutoff=.001)
#we used a cutoff of 0.001 f or the pvalue. And now we can inspect the Dag object
sigCategories(DAVIDGODag_EdgeR, p=0.0001)
```

NULL

```r
summary(DAVIDGODag_EdgeR)
```

```
## Warning: No results met the specified criteria. Returning 0-row data.frame
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["GOBPID"],"name":[1],"type":["chr"],"align":["left"]},{"label":["Pvalue"],"name":[2],"type":["dbl"],"align":["right"]},{"label":["Count"],"name":[3],"type":["int"],"align":["right"]},{"label":["Size"],"name":[4],"type":["int"],"align":["right"]},{"label":["Term"],"name":[5],"type":["chr"],"align":["left"]}],"data":[],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

```r
show(DAVIDGODag_EdgeR)
```

Gene to GO BP  test for over-representation 
0 GO BP ids tested (0 have p < 0.001)
Selected gene set size: 0 

```
## Warning in max(popTotals(r)): no non-missing arguments to max; returning -
## Inf
```

    Gene universe size: -Inf 
    Annotation package: DAVID.website 

```r
# we can see by the FDR column that only the first 3 pathway are significant
```

## The graph using the EdgeR data


```r
library(Rgraphviz)
plotGOTermGraph(g=goDag(DAVIDGODag_EdgeR), r= DAVIDGODag_EdgeR, max.nchar= 20, node.shape="ellipse",node.colors=c(sig="red", not="white"))
```

```
## Warning in .local(x, y, ...): graph has zero nodes; cannot layout
```


## Comparison between the DESeq2 and EdgeR results

Venndiagram of total genes:


```r
library(VennDiagram)
# Total DE genes 
I_total<- intersect(rownames(resSig_Salmon), rownames(Topgenes_sfactor_edgeR_0.1_1$table))
length(I_total)
```

[1] 350

```r
grid.newpage()
draw.pairwise.venn(nrow(resSig_Salmon), nrow(Topgenes_sfactor_edgeR_0.1_1$table), length(I_total), category = c("DESeq2", "EdgeR"), lty = rep("blank", 
    2), fill = c("light blue", "pink"), alpha = rep(0.5, 2), cat.pos = c(-15, 
    0), cat.dist = rep(0.025, 2), title= "DE genes from DESeq and EdgeR")
```

![](C:/Users/rpmello/Desktop/Data Analysis RNASeq/Projeto_Salmon_DESeq_EdgeR/Output/FFD_vs_Goat_milkD_Salmon_R_with_outliers/Figuresintersect_DESeq_EdgeR-1.png)<!-- -->(polygon[GRID.polygon.81650], polygon[GRID.polygon.81651], polygon[GRID.polygon.81652], polygon[GRID.polygon.81653], text[GRID.text.81654], text[GRID.text.81655], lines[GRID.lines.81656], text[GRID.text.81657], text[GRID.text.81658], text[GRID.text.81659]) 

### Comparison between the downregulated genes

```r
I_down<- intersect(rownames(downresSig_Salmon), rownames(Topgenes_sfactor_edgeR_0.1_1_down$table))
length(I_down)
```

[1] 2

```r
grid.newpage()
draw.pairwise.venn(nrow(downresSig_Salmon), nrow(Topgenes_sfactor_edgeR_0.1_1_down$table), length(I_down), category = c("DESeq2", "EdgeR"), lty = rep("blank", 
    2), fill = c("light blue", "pink"), alpha = rep(0.5, 2), cat.pos = c(-15, 
    0), cat.dist = rep(0.025, 2), title= "Down regulated genes according to DESeq and/or EdgeR")
```

![](C:/Users/rpmello/Desktop/Data Analysis RNASeq/Projeto_Salmon_DESeq_EdgeR/Output/FFD_vs_Goat_milkD_Salmon_R_with_outliers/Figuresdown_venndiagram-1.png)<!-- -->(polygon[GRID.polygon.81660], polygon[GRID.polygon.81661], polygon[GRID.polygon.81662], polygon[GRID.polygon.81663], text[GRID.text.81664], text[GRID.text.81665], text[GRID.text.81666], text[GRID.text.81667]) 

### The upregulated genes 

```r
I_up<- intersect(rownames(upresSig_Salmon), rownames(Topgenes_sfactor_edgeR_0.1_1_up$table))
length(I_up)
```

[1] 348

```r
grid.newpage()
draw.pairwise.venn(nrow(upresSig_Salmon), nrow(Topgenes_sfactor_edgeR_0.1_1_up$table), length(I_up), category = c("DESeq2", "EdgeR"), lty = rep("blank", 
    2), fill = c("light blue", "pink"), alpha = rep(0.5, 2), cat.pos = c(-15, 
    0), cat.dist = rep(0.025, 2), title= "up DE genes from DESeq and EdgeR")
```

![](C:/Users/rpmello/Desktop/Data Analysis RNASeq/Projeto_Salmon_DESeq_EdgeR/Output/FFD_vs_Goat_milkD_Salmon_R_with_outliers/Figuresvenn_up-1.png)<!-- -->(polygon[GRID.polygon.81668], polygon[GRID.polygon.81669], polygon[GRID.polygon.81670], polygon[GRID.polygon.81671], text[GRID.text.81672], text[GRID.text.81673], text[GRID.text.81674], text[GRID.text.81675], text[GRID.text.81676]) 

## Get the list of the genes that are in both DESeq2  and edgeR DE genes.


```r
head(I_total)
```

[1] "ENSSSCG00000010488" "ENSSSCG00000003006" "ENSSSCG00000032158"
[4] "ENSSSCG00000035347" "ENSSSCG00000011629" "ENSSSCG00000002009"

```r
Salmon_intersect<-gene_names[rownames(gene_names) %in% I_total, ]
saveRDS(Salmon_intersect,file = file.path(output_dir,sprintf("%s_vs_%s_Salmon_intersect_w_outliers.RDS",groups[1], groups[2])))
```
###### Session info

```r
sessionInfo()
```

R version 3.6.0 (2019-04-26)
Platform: i386-w64-mingw32/i386 (32-bit)
Running under: Windows >= 8 x64 (build 9200)

Matrix products: default

locale:
[1] LC_COLLATE=English_United States.1252 
[2] LC_CTYPE=English_United States.1252   
[3] LC_MONETARY=English_United States.1252
[4] LC_NUMERIC=C                          
[5] LC_TIME=English_United States.1252    

attached base packages:
 [1] grid      stats4    parallel  stats     graphics  grDevices utils    
 [8] datasets  methods   base     

other attached packages:
 [1] RDAVIDWebService_1.22.0     VennDiagram_1.6.20         
 [3] futile.logger_1.4.3         org.Hs.eg.db_3.8.2         
 [5] ReactomePA_1.28.0           GOplot_1.0.2               
 [7] gridExtra_2.3               ggdendro_0.1-20            
 [9] Rgraphviz_2.28.0            GO.db_3.8.2                
[11] genefilter_1.66.0           biomaRt_2.40.0             
[13] pheatmap_1.0.12             gplots_3.0.1.1             
[15] PoiClaClu_1.0.2.1           RUVSeq_1.18.0              
[17] edgeR_3.26.0                limma_3.40.0               
[19] RColorBrewer_1.1-2          EDASeq_2.18.0              
[21] ShortRead_1.42.0            GenomicAlignments_1.20.0   
[23] Rsamtools_2.0.0             Biostrings_2.52.0          
[25] XVector_0.24.0              DESeq2_1.24.0              
[27] SummarizedExperiment_1.14.0 DelayedArray_0.10.0        
[29] BiocParallel_1.17.18        matrixStats_0.54.0         
[31] GenomicRanges_1.36.0        GenomeInfoDb_1.20.0        
[33] tximport_1.12.0             knitr_1.22                 
[35] ggplot2_3.1.1               GOstats_2.50.0             
[37] Category_2.50.0             Matrix_1.2-17              
[39] AnnotationDbi_1.46.0        IRanges_2.18.0             
[41] S4Vectors_0.22.0            Biobase_2.44.0             
[43] graph_1.62.0                BiocGenerics_0.30.0        

loaded via a namespace (and not attached):
  [1] splines_3.6.0          ggplotify_0.0.3        urltools_1.7.3        
  [4] bitops_1.0-6           tibble_2.1.1           R.oo_1.22.0           
  [7] polyclip_1.10-0        triebeard_0.3.0        rpart_4.1-15          
 [10] XML_3.98-1.19          MASS_7.3-51.4          lattice_0.20-38       
 [13] magrittr_1.5           backports_1.1.4        rmarkdown_1.12        
 [16] Hmisc_4.2-0            yaml_2.2.0             cowplot_0.9.4         
 [19] DBI_1.0.0              zlibbioc_1.30.0        rvcheck_0.1.3         
 [22] R.utils_2.8.0          purrr_0.3.2            RCurl_1.95-4.12       
 [25] ggraph_1.0.2           nnet_7.3-12            rappdirs_0.3.1        
 [28] tweenr_1.0.1           GenomeInfoDbData_1.2.1 enrichplot_1.4.0      
 [31] ggrepel_0.8.0          AnnotationForge_1.26.0 gdata_2.18.0          
 [34] acepack_1.4.1          reactome.db_1.68.0     annotate_1.62.0       
 [37] xml2_1.2.0             ggforce_0.2.2          DOSE_3.10.0           
 [40] tidyselect_0.2.5       farver_1.1.0           viridis_0.5.1         
 [43] base64enc_0.1-3        DO.db_2.9              jsonlite_1.6          
 [46] Formula_1.2-3          survival_2.44-1.1      ggridges_0.5.1        
 [49] progress_1.2.0         tools_3.6.0            Rcpp_1.0.1            
 [52] glue_1.3.1             xfun_0.6               qvalue_2.16.0         
 [55] dplyr_0.8.0.1          withr_2.1.2            formatR_1.6           
 [58] latticeExtra_0.6-28    digest_0.6.18          caTools_1.17.1.2      
 [61] gridGraphics_0.4-0     R6_2.4.0               colorspace_1.4-1      
 [64] gtools_3.8.1           RSQLite_2.1.1          R.methodsS3_1.7.1     
 [67] UpSetR_1.3.3           data.table_1.12.2      rtracklayer_1.44.0    
 [70] prettyunits_1.0.2      htmlwidgets_1.3        httr_1.4.0            
 [73] graphite_1.30.0        pkgconfig_2.0.2        gtable_0.3.0          
 [76] rJava_0.9-11           blob_1.1.1             hwriter_1.3.2         
 [79] htmltools_0.3.6        fgsea_1.10.0           geneplotter_1.62.0    
 [82] RBGL_1.60.0            GSEABase_1.46.0        scales_1.0.0          
 [85] lambda.r_1.2.3         rstudioapi_0.10        reshape2_1.4.3        
 [88] curl_3.3               checkmate_1.9.3        stringr_1.4.0         
 [91] KernSmooth_2.23-15     foreign_0.8-71         pillar_1.3.1          
 [94] cluster_2.0.9          xtable_1.8-4           htmlTable_1.13.1      
 [97] evaluate_0.13          GenomicFeatures_1.36.0 readr_1.3.1           
[100] futile.options_1.0.1   compiler_3.6.0         locfit_1.5-9.1        
[103] crayon_1.3.4           rlang_0.3.4            labeling_0.3          
[106] aroma.light_3.14.0     plyr_1.8.4             stringi_1.4.3         
[109] viridisLite_0.3.0      assertthat_0.2.1       munsell_0.5.0         
[112] lazyeval_0.2.2         GOSemSim_2.10.0        europepmc_0.3         
[115] hms_0.4.2              bit64_0.9-7            highr_0.8             
[118] DESeq_1.36.0           igraph_1.2.4.1         memoise_1.1.0         
[121] fastmatch_1.1-0        bit_1.1-14            
