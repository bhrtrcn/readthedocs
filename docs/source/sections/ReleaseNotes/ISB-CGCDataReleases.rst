############################
ISB-CGC Data Release Notes
############################

Introduction text



**August 21, 2016:**

New **miRBase_v21** table added to the **isb-cgc:genome_reference** BigQuery dataset.

**August 20, 2016:** 

Updated **hg19** and **hg38** `Kaviar <http://db.systemsbiology.net/kaviar/>`_ tables added to the **isb-cgc:genome_reference** BigQuery dataset.

**August 17, 2016:** 

New **isb-cgc:GDC_metadata** BigQuery dataset containing metadata for both *legacy* and *current* files hosted at the `NCI-GDC <https://gdc.cancer.gov/>`_.

**July 28, 2016:** 

New **isb-cgc:tcga_201607_beta** BigQuery dataset based on the *final* TCGA data upload from the DCC.  This dataset largely mirrors the previous **isb-cgc:tcga_20510_alpha** dataset and is now also supporting the ISB-CGC Web-App.  The curated TCGA cohort tables in the **isb-cgc:tcga_cohorts** BigQuery dataset have also been updated.

**June 24, 2016:** 

An updated listing of all ISB-CGC hosted data in Google Cloud Storage (GCS) is now available in the **GCS_listing_24jun2016** table in the **isb-cgc:tcga_seq_metadata** dataset in BigQuery, in addition the **CGHub_Manifest_24jun2016** table contains the final CGHub Manifest prior to the transition of all data to the `Genomic Data Commons <https://portal.gdc.cancer.gov/>`_.

**June 18, 2016:** 

New **GENCODE_r24** table added to the **isb-cgc:genome_reference** BigQuery dataset.

**May 13, 2016:** 

New **NCBI_Viral_Annotations_Taxid10239** table added to the **isb-cgc:genome_reference** BigQuery dataset.

**May 9, 2016:** 

New **Ensembl2Reactome** and **miRBase2Reactome** tables added to the **isb-cgc:genome_reference** BigQuery dataset.

**May 3, 2016:**

New **isb-cgc:tcga_seq_metadata** BigQuery dataset contains metadata and FastQC metrics for thousands of TCGA DNA-seq and RNA-seq data files:
- **CGHub_Manifest** table contains metadata for all TCGA files at CGHub as of April 27th, 2016
- **GCS_listing_27apr2016** table contains metadata for all TCGA files hosted by ISB-CGC in GCS 
- **RNAseq_FastQC** table contains metrics derived from FastQC runs on the RNAseq data files, including urls to the FastQC html reports that you can cut and paste directly into your browser
- **WXS_FastQC** table contains metrics derived from FastQC runs on the exome DNAseq data files

**April 28, 2016**

**GO_Ontology** and **GO_Annotations** tables added to the **isb-cgc:genome_reference** BigQuery dataset.

**March 14, 2016**

With the release of our **Web-App**, controlled-data is now accessible (programmatically) to users who have previously obtained dbGaP approval for TCGA data and go through the NIH authentication process built-in to the Web-App.

**February 26, 2016**

New CCLE dataset in BigQuery **isb-cgc:ccle_201602_alpha** includes sample metadata, mutation calls, copy-number segments, and expression data (metadata includes full cloud-storage-path for world-readable BAM and SNP CEL files, and Genomics dataset- and readgroupset-ids for sequence data imported into Google Genomics).

**February 22, 2016**

Kaviar database now available in the **isb-cgc:genome_reference** BigQuery dataset.

**February 19, 2016**

CCLE RNAseq and DNAseq bam files imported into **Google Genomics**.

**January 10, 2016**

**GENCODE_r19** and **miRBase_v20** tables added to the **isb-cgc:genome_reference** BigQuery dataset.

**December 26, 2015**

Public release of new **isb-cgc:genome_reference** BigQuery dataset: the first table is based on the just-published **miRTarBase** release 6.1.

**December, 12, 2015**

Curated TCGA cohort lists available in **isb-cgc:tcga_cohorts** BigQuery dataset.

**December 3, 2015**

Version `v0.1 <https://github.com/isb-cgc/ISB-CGC-Webapp/releases/tag/1.0>`_.

First tagged release of the web-app.

**November 16, 2015**

Initial upload of data from CGHub into **Google Cloud Storage** (GCS) complete (not publicly released).

**November 2, 2015**

First public release of TCGA open-access data in BigQuery tables.

- **isb-cgc:tcga_201510_alpha** dataset contains updated set of BigQuery tables, based on data available at the TCGA DCC as of October 2015
- Includes **Annotations** table with information about redacted samples, etc
- **isb-cgc:platform_reference** contains annotation information for the Illumina DNA Methylation platform

**October 4, 2015**

Complete data upload from TCGA DCC, including controlled-access data

**September 21, 2015** 

Draft set of BigQuery tables (not publicly released)

- **isb-cgc:tcga_201507_alpha** dataset containing clinical, biospecimen, somatic mutation calls and Level-3 TCGA data available at the TCGA DCC as of July 2015
