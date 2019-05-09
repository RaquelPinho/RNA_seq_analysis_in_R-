# RNAseq_analysis_in_R
In this repository, I am analyzing the data from the three sections of small intestines using RUVseq package to account for the unwanted variance in the data, amd used both  DESeq2 (MLE) and EdgeR (glmQLF) to calculate the DE genes  in both softwares. Also I am comparing Kallisto and Salmon counts.

## Salmon counts

The code used to trim, do the quality analysis, mapping and count for the this read can be found [here](githubwithcodesalmon)

### Pairwise comparisons within intestinal sections

#### Duodenum

##### Relevant comparisons

[Full Fed Group vs Malnourished Group](http://htmlpreview.github.io/? htmlfolder)

[Full Fed Group vs All Malnourished Groups](http://htmlpreview.github.io/? htmlfolder)

[Malnourished Group vs Goat milk Group](http://htmlpreview.github.io/? htmlfolder)

[Malnourished Group vs hLZ milk Group](http://htmlpreview.github.io/? htmlfolder)

##### Other comparisons

[Full Fed Group vs vs Goat milk Group](http://htmlpreview.github.io/? htmlfolder)

[Full Fed Group vs All hLZ milk Group](http://htmlpreview.github.io/?htmlfolder)

[Goat milk Group vs hLZ milk Group](http://htmlpreview.github.io/? htmlfolder)

#### Jejunum

##### Relevant comparisons

[Full Fed Group vs Malnourished Group](http://htmlpreview.github.io/? htmlfolder)

[Full Fed Group vs All Malnourished Groups](http://htmlpreview.github.io/? htmlfolder)

[Malnourished Group vs Goat milk Group](http://htmlpreview.github.io/? htmlfolder)

[Malnourished Group vs hLZ milk Group](http://htmlpreview.github.io/? htmlfolder)

##### Other comparisons

[Full Fed Group vs vs Goat milk Group](http://htmlpreview.github.io/? htmlfolder)

[Full Fed Group vs All hLZ milk Group](http://htmlpreview.github.io/? htmlfolder)

[Goat milk Group vs hLZ milk Group](http://htmlpreview.github.io/? htmlfolder)

#### Ileum

##### Relevant comparisons

[Full Fed Group vs Malnourished Group](http://htmlpreview.github.io/? htmlfolder)

[Full Fed Group vs All Malnourished Groups](http://htmlpreview.github.io/? htmlfolder)

[Malnourished Group vs Goat milk Group](http://htmlpreview.github.io/? htmlfolder)

[Malnourished Group vs hLZ milk Group](link htmlfolder)

##### Other comparisons

[Full Fed Group vs vs Goat milk Group](http://htmlpreview.github.io/? htmlfolder)

[Full Fed Group vs hLZ milk Group](http://htmlpreview.github.io/? htmlfolder)

[Goat milk Group vs hLZ milk Group](http://htmlpreview.github.io/? htmlfolder)

### Along the length of the intestines

[Full Fed Group Duodenum vs Full Fed Group Jejunum](http://htmlpreview.github.io/? htmlfolder)

[Full Fed Group Duodenum vs Full Fed Group Ileum](http://htmlpreview.github.io/? htmlfolder)

[Full Fed Group Jejunum vs Full Fed Group Ileum](http://htmlpreview.github.io/? htmlfolder)

[Malnourished Group Duodenum vs Malnourished Group Jejunum](http://htmlpreview.github.io/? htmlfolder)

[Malnourished Group Duodenum vs Malnourished Group Ileum](http://htmlpreview.github.io/? htmlfolder)

[Malnourished Group Jejunum vs Malnourished Group Ileum](http://htmlpreview.github.io/? htmlfolder)

[Goat milk Group Duodenum vs Goat milk Group Jejunum](http://htmlpreview.github.io/? htmlfolder)

[Goat milk Group Duodenum vs Goat milk Group Ileum](http://htmlpreview.github.io/? htmlfolder)

[Goat milk Group Jejunum vs Goat milk Group Ileum](http://htmlpreview.github.io/? htmlfolder)

[hLZ milk Group Duodenum vs hLZ milk Group Jejunum](http://htmlpreview.github.io/? htmlfolder)

[hLZ milk Group Duodenum vs hLZ milk Group Ileum](http://htmlpreview.github.io/? htmlfolder)

[hLZ milk Group Jejunum vs hLZ milk Group Ileum](http://htmlpreview.github.io/? htmlfolder)

#### All groups
[Duodenum vs Jejunum](http://htmlpreview.github.io/? htmlfolder)

[Duodenum vs leum](http://htmlpreview.github.io/? htmlfolder)

[Jejunum vs Ileum](http://htmlpreview.github.io/? htmlfolder)

### General comparisons

[Full Fed All sections vs Malnourished All sections](http://htmlpreview.github.io/? htmlfolder)

## Kallisto counts

The code used to trim, do the quality analysis, mapping and count for the this read can be found [here](githubwithcodesalmon)

### Pairwise comparisons within intestinal sections

#### Duodenum

##### Relevant comparisons

[Full Fed Group vs Malnourished Group](http://htmlpreview.github.io/? htmlfolder)

[Full Fed Group vs All Malnourished Groups](http://htmlpreview.github.io/? htmlfolder)

[Malnourished Group vs Goat milk Group](http://htmlpreview.github.io/? htmlfolder)

[Malnourished Group vs hLZ milk Group](http://htmlpreview.github.io/? htmlfolder)

##### Other comparisons

[Full Fed Group vs vs Goat milk Group](http://htmlpreview.github.io/? htmlfolder)

[Full Fed Group vs All hLZ milk Group](http://htmlpreview.github.io/?htmlfolder)

[Goat milk Group vs hLZ milk Group](http://htmlpreview.github.io/? htmlfolder)

#### Jejunum

##### Relevant comparisons

[Full Fed Group vs Malnourished Group](http://htmlpreview.github.io/? htmlfolder)

[Full Fed Group vs All Malnourished Groups](http://htmlpreview.github.io/? htmlfolder)

[Malnourished Group vs Goat milk Group](http://htmlpreview.github.io/? htmlfolder)

[Malnourished Group vs hLZ milk Group](http://htmlpreview.github.io/? htmlfolder)

##### Other comparisons

[Full Fed Group vs vs Goat milk Group](http://htmlpreview.github.io/? htmlfolder)

[Full Fed Group vs All hLZ milk Group](http://htmlpreview.github.io/? htmlfolder)

[Goat milk Group vs hLZ milk Group](http://htmlpreview.github.io/? htmlfolder)

#### Ileum

##### Relevant comparisons

[Full Fed Group vs Malnourished Group](http://htmlpreview.github.io/? htmlfolder)

[Full Fed Group vs All Malnourished Groups](http://htmlpreview.github.io/? htmlfolder)

[Malnourished Group vs Goat milk Group](http://htmlpreview.github.io/? htmlfolder)

[Malnourished Group vs hLZ milk Group](link htmlfolder)

##### Other comparisons

[Full Fed Group vs vs Goat milk Group](http://htmlpreview.github.io/? htmlfolder)

[Full Fed Group vs hLZ milk Group](http://htmlpreview.github.io/? htmlfolder)

[Goat milk Group vs hLZ milk Group](http://htmlpreview.github.io/? htmlfolder)

### Along the length of the intestines

[Full Fed Group Duodenum vs Full Fed Group Jejunum](http://htmlpreview.github.io/? htmlfolder)

[Full Fed Group Duodenum vs Full Fed Group Ileum](http://htmlpreview.github.io/? htmlfolder)

[Full Fed Group Jejunum vs Full Fed Group Ileum](http://htmlpreview.github.io/? htmlfolder)

[Malnourished Group Duodenum vs Malnourished Group Jejunum](http://htmlpreview.github.io/? htmlfolder)

[Malnourished Group Duodenum vs Malnourished Group Ileum](http://htmlpreview.github.io/? htmlfolder)

[Malnourished Group Jejunum vs Malnourished Group Ileum](http://htmlpreview.github.io/? htmlfolder)

[Goat milk Group Duodenum vs Goat milk Group Jejunum](http://htmlpreview.github.io/? htmlfolder)

[Goat milk Group Duodenum vs Goat milk Group Ileum](http://htmlpreview.github.io/? htmlfolder)

[Goat milk Group Jejunum vs Goat milk Group Ileum](http://htmlpreview.github.io/? htmlfolder)

[hLZ milk Group Duodenum vs hLZ milk Group Jejunum](http://htmlpreview.github.io/? htmlfolder)

[hLZ milk Group Duodenum vs hLZ milk Group Ileum](http://htmlpreview.github.io/? htmlfolder)

[hLZ milk Group Jejunum vs hLZ milk Group Ileum](http://htmlpreview.github.io/? htmlfolder)

#### All groups
[Duodenum vs Jejunum](http://htmlpreview.github.io/? htmlfolder)

[Duodenum vs leum](http://htmlpreview.github.io/? htmlfolder)

[Jejunum vs Ileum](http://htmlpreview.github.io/? htmlfolder)

### General comparisons

[Full Fed All sections vs Malnourished All sections](http://htmlpreview.github.io/? htmlfolder)


### Comparison between Salmon and Kallisto results

#### Duodenum

##### Relevant comparisons

[Full Fed Group vs Malnourished Group](http://htmlpreview.github.io/? htmlfolder)

[Full Fed Group vs All Malnourished Groups](http://htmlpreview.github.io/? htmlfolder)

[Malnourished Group vs Goat milk Group](http://htmlpreview.github.io/? htmlfolder)

[Malnourished Group vs hLZ milk Group](http://htmlpreview.github.io/? htmlfolder)

##### Other comparisons

[Full Fed Group vs vs Goat milk Group](http://htmlpreview.github.io/? htmlfolder)

[Full Fed Group vs All hLZ milk Group](http://htmlpreview.github.io/?htmlfolder)

[Goat milk Group vs hLZ milk Group](http://htmlpreview.github.io/? htmlfolder)

#### Jejunum

##### Relevant comparisons

[Full Fed Group vs Malnourished Group](http://htmlpreview.github.io/? htmlfolder)

[Full Fed Group vs All Malnourished Groups](http://htmlpreview.github.io/? htmlfolder)

[Malnourished Group vs Goat milk Group](http://htmlpreview.github.io/? htmlfolder)

[Malnourished Group vs hLZ milk Group](http://htmlpreview.github.io/? htmlfolder)

##### Other comparisons

[Full Fed Group vs vs Goat milk Group](http://htmlpreview.github.io/? htmlfolder)

[Full Fed Group vs All hLZ milk Group](http://htmlpreview.github.io/? htmlfolder)

[Goat milk Group vs hLZ milk Group](http://htmlpreview.github.io/? htmlfolder)

#### Ileum

##### Relevant comparisons

[Full Fed Group vs Malnourished Group](http://htmlpreview.github.io/? htmlfolder)

[Full Fed Group vs All Malnourished Groups](http://htmlpreview.github.io/? htmlfolder)

[Malnourished Group vs Goat milk Group](http://htmlpreview.github.io/? htmlfolder)

[Malnourished Group vs hLZ milk Group](link htmlfolder)

##### Other comparisons

[Full Fed Group vs vs Goat milk Group](http://htmlpreview.github.io/? htmlfolder)

[Full Fed Group vs hLZ milk Group](http://htmlpreview.github.io/? htmlfolder)

[Goat milk Group vs hLZ milk Group](http://htmlpreview.github.io/? htmlfolder)

### Along the length of the intestines

[Full Fed Group Duodenum vs Full Fed Group Jejunum](http://htmlpreview.github.io/? htmlfolder)

[Full Fed Group Duodenum vs Full Fed Group Ileum](http://htmlpreview.github.io/? htmlfolder)

[Full Fed Group Jejunum vs Full Fed Group Ileum](http://htmlpreview.github.io/? htmlfolder)

[Malnourished Group Duodenum vs Malnourished Group Jejunum](http://htmlpreview.github.io/? htmlfolder)

[Malnourished Group Duodenum vs Malnourished Group Ileum](http://htmlpreview.github.io/? htmlfolder)

[Malnourished Group Jejunum vs Malnourished Group Ileum](http://htmlpreview.github.io/? htmlfolder)

[Goat milk Group Duodenum vs Goat milk Group Jejunum](http://htmlpreview.github.io/? htmlfolder)

[Goat milk Group Duodenum vs Goat milk Group Ileum](http://htmlpreview.github.io/? htmlfolder)

[Goat milk Group Jejunum vs Goat milk Group Ileum](http://htmlpreview.github.io/? htmlfolder)

[hLZ milk Group Duodenum vs hLZ milk Group Jejunum](http://htmlpreview.github.io/? htmlfolder)

[hLZ milk Group Duodenum vs hLZ milk Group Ileum](http://htmlpreview.github.io/? htmlfolder)

[hLZ milk Group Jejunum vs hLZ milk Group Ileum](http://htmlpreview.github.io/? htmlfolder)

#### All groups
[Duodenum vs Jejunum](http://htmlpreview.github.io/? htmlfolder)

[Duodenum vs leum](http://htmlpreview.github.io/? htmlfolder)

[Jejunum vs Ileum](http://htmlpreview.github.io/? htmlfolder)

### General comparisons

[Full Fed All sections vs Malnourished All sections](http://htmlpreview.github.io/? htmlfolder)



[FF vs Mall all groups](https://htmlpreview.github.io/?https://github.com/RaquelPinho/RNAseq_analyzis_in_R/blob/master/Salmon_counts/RUV_FF_vs_Mal_all_groups_all_sections_Salmon.html)


[FF_vs_Goat_milk](http://htmlpreview.github.io/?https://github.com/RaquelPinho/RNAseq_analyzis_in_R/blob/master/Salmon_counts/FFI_VS_GoatI/FFI_vs_Goat_milkI_Salmon_R_with_outliers.html#4_edger_analysis)
