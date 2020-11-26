# Statistical Analysis of eDNA Results

We will firstly load the qiime2R library to load the data output from Qiime2. 
We will use the betapart library for diversity analyses, and ggfortify for plotting.  

```r
library(qiime2R)
library(betapart)
library(ggfortify)
```


We load our Qiime2 results using read_qza. 
```r
data = read_qza('/nesi/project/nesi02659/obss_2020/resources/day5/other_resources/stats/fish_freq_table.qza')
otus = read_qza('/nesi/project/nesi02659/obss_2020/resources/day5/other_resources/stats/zotus.qza')
```

Let's see what information we get about the respective OTUs:
```r
print(otus)
class(otus)
otus$data
```

We investigate our data structure, and extract the read count table that we are interested in.
```r
print(data)
class(data)
dataf = data$data
class(dataf)
```

We now load a pre-prepared file that maps the OTUs in our frequency table to the respective taxonomies; we will then collapse OTUs that are assigned to the same species.  
```r

# Currently our table only has OTUs as rownames:
rownames(dataf)
# We want to substitute those with actual taxonomies!

# We can load the taxonomy output delivered by Qiime2:
annot = read.table('/nesi/project/nesi02659/obss_2020/resources/day5/other_resources/stats/fish_OTUs_NB_taxonomy.tsv')
# This results in an error - why? 
annot = read.table('/nesi/project/uoo02328/eDNA2020/fishtest1/fish_OTUs_NB_taxonomy.tsv', sep='\t', header=TRUE, row.names=1)

# Look at an exemplary taxonomy annotation:
annot$Taxon[1]

# Process the annotation (one example) to only get the genus level annotation:
strsplit(annot$Taxon[1], ";g__")
class(strsplit(annot$Taxon[1], ";g__"))
unlist(strsplit(annot$Taxon[1], ";g__"))[2]
unlist(strsplit(unlist(strsplit(annot$Taxon[1], ";g__"))[2], '_'))[1]

# Apply this function to all annotations with the help of a function:
f = function(x) unlist(strsplit(unlist(strsplit(x, ";g__"))[2], '_'))[1]
sapply(annot$Taxon, f, USE.NAMES = FALSE)

# Why do we observe NAs?

# Subsitute OTU row names with genus names
rownames(dataf) = sapply(annot$Taxon, f, USE.NAMES = FALSE)

# Delete NAs:
dataf = dataf[!rownames(dataf) %in% NA, ]

# Aggregate reads per genus:
dataf = rowsum(dataf, row.names(dataf))

```

We can now either transform the data to presence/absence data, and exclude singleton (OTUs with only one read count):
```r
dataf_bin = dataf
dataf_bin[dataf==1] = 0 
dataf_bin[dataf>0] = 1
```

or "rarefy" the data: 
```r

# look at number of reads per samples
colSums(dataf)
min(colSums(dataf))
min_reads = min(colSums(dataf))

# create rarefied table
dataf_rar = dataf
for (i in 1:dim(dataf)[2]){
  dataf_rar[,i] = rmultinom(n = 1, size = min_reads, prob = dataf[,i])
}
```

How would you calculate the alpha diversity of these samples? 
```r
colSums(dataf_bin)
# or
sum(dataf_rar[,1] != 0)
```

We will now apply some standard biodiversity calculations using the betapart package; what do these results mean? 
```r
betar = beta.multi(dataf_bin, index.family = "jaccard")
print(betar$beta.JAC)
```

We can compare the abundance of the taxa between the sample types: 
```r
boxplot(dataf_rar['Aldrichetta',1:6], dataf_rar['Aldrichetta',7:11])

boxplot(dataf_rar['Aldrichetta',1:6], dataf_rar['Aldrichetta',7:11], notch = TRUE,
  ylab = 'DNA read count', xlab = 'Aldrichetta', names = c('M', 'R'))

```

We will now plot a PCA to visualise the relationship between the samples; for that we will firstly have to transpose the data so that the samples are in the rows:
```r
dataf_rar = t(dataf_rar)

dataf_rar_sc = scale(dataf_rar)

pca <- prcomp(dataf_rar_sc, scale. = TRUE)
autoplot
autoplot(pca, label = TRUE)
autoplot(pca, label = TRUE, shape = FALSE, label.size = 3)

autoplot(pca)
autoplot(pca, loadings = TRUE, loadings.colour = 'blue',
         loadings.label = TRUE, loadings.label.size = 3)
```

A heatmap directly clusters the samples and shows their relationship: 
```r
dist = dist(dataf_rar_sc)
autoplot(dist)
```

