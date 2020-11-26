# Preparing a Reference Database

The following are the steps used to prepare the mitochondrial lrRNA database for this project. Please keep in mind that if sequences are taken from an uncurated database such as NCBI or EMBL, some additional work would be needed. 

First, import the sequence and taxonomy files into Qiime:



The following steps can take a good deal of resources, depending on the size of the database. We recommend to run all of these as job scripts. Some example job scripts are included in the scripts folder. 

The next step is suggested for some genes (16S bacteria), but not for others (fungal ITS). However, we are doing this here to help with accuracy. This will edit each sequence so that just the part of the sequence around the primers used in the PCR remains. 

```
REFDIR='/nesi/project/uoo02328/metabarcoding/refs'
qiime feature-classifier extract-reads \
  --i-sequences ${REFDIR}/MIDORI_UNIQ_GB239_lrRNA_QIIME_Ref_Seqs.qza \
  --p-f-primer GACCCTATGGAGCTTTAGAC \
  --p-r-primer CGCTGTTATCCCTADRGTAATC \
  --p-min-length 140 \
  --p-max-length 300 \
  --p-identity 0.8 \
  --o-reads MIDORI_UNIQ_GB239_lrRNA_fishQiiExt_QIIME_Ref.qza \
  --p-n-jobs 16
```

The extracted sequence file and the taxonomy file imported above can now be used for taxonomy assignment for BLAST and vsearch. For Naive Bayes machine learning, an additional step is needed. The sequences need to be 'trained'; this is the part that involves machine learning, as the sequences are teaching the program what parts of the sequence are diagnostic. For large databases, this can take a long time. 

```
REFDIR='/nesi/project/uoo02328/metabarcoding/refs'
qiime feature-classifier fit-classifier-naive-bayes \
 --i-reference-reads MIDORI_UNIQ_GB239_lrRNA_crustQiiExt_QIIME_Ref.qza \
 --i-reference-taxonomy ${REFDIR}/MIDORI_UNIQ_GB239_lrRNA_QIIME_taxon.qza  \
 --o-classifier MIDORI_UNIQ_GB239_lrRNA_crustQiiExt_classifier.qza \
 --p-classify--chunk-size 10000
```

## Additional step: dereplicate the database

Much of the database curation literature also recommends to dereplicate the database. This means that for each species, you exclude all identical sequences but one. This will help searches to find all species matching a query. Note, that to run this, you have to export the extracted sequences out of Qiime format, run the dereplication, and then reimport the dereplicated sequences into Qiime.

One program that can run this operation is [**MetaCurator**](https://github.com/RTRichar/MetaCurator). This program includes the script `DerepByTaxonomy.py`. This was run on our database with the following command:

```
module load Python/3.8.2-gimkl-2020a
module load VSEARCH/2.14.2-GCC-9.2.0

/PATH/TO/SCRIPT/DerepByTaxonomy.py -p 16 \
 -i MIDORI_UNIQ_GB239_lrRNA_fishQiiExt_QIIME_RefSeq.fasta \
 -t MIDORI_UNIQ_GB239_lrRNA_QIIME.taxon \
 -o dr_MIDORI_UNIQ_GB239_lrRNA_fish_QIIME_RefSeq.fasta
```

