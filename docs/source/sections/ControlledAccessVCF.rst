Variant Data in BigQuery
************************


Accessing Controlled Variant Data 
=================================
Some ISB-CGC Bigquery tables contain sensitive information about patients. These type of files are known as controlled access files. To obtain access to our controlled data please follow the steps in our `Accessing Controlled Data <https://isb-cancer-genomics-cloud.readthedocs.io/en/latest/sections/Gaining-Access-To-Controlled-Access-Data.html>`_ page to obtain permission.   



Variant Call Format (VCF)
========================

Variant Call Format files (VCF) are the standard file format to store identified variants within sequenced data. The creation of VCF files start from sequencing whole genomes (WGS) or whole exome sequencing (WXS) that create FASTQ files. The file containing the sequenced genome is then aligned to the appropriate reference genome which then generates a SAM, BAM, or CRAM files. The last step to generate the VCF file from either of the three alignment files, the differing aligned reads will be identified when comparing to the reference genome and written out to a VCF file.

As variant data is increasing and growing in size researchers face the problem of being able to analyze all the new data arriving as well as the old data. The new and old VCF files that are being curated by all these programs (Ex. TCGA,TARGET, and FM) are stored as individual files on local computers or on High Performance Computing (HPCs) for download. Researchers face the problem of analyzing these large scale data at once to gain insights for the analysis they are running. 

Variant Data to BigQuery Tables
===============================

ISB-CGC has developed an ETL pipeline to take controlled access VCF files found at Genomic Data Commons (GDC). This transforms pipeline is able to take the terabytes of variant data found at GDC and transform them into one large and flattened Google BigQuery table. The transforms pipeline ISB-CGC has developed allows researchers to query, use command line tools, or use a programming language of your choice to gain statistical insights of an analysis someone might be interested in. 


Flattened VCF BigQuery Table
============================

.. figure:: BigQuery_VCF_Flattened.png 
   :scale: 50
   :align: center 



SQL Query Examples 
===================

Notes to include examples on caveats: 
POS is a integer, so in sql query don't use the quotes 

In this query, let's find all information for patients who have ALL-P2 and a Thymine mutation at position 161550724 on Chromosome 1. 
.. code-block:: sql

   SELECT * FROM `isb-cgc-etl.STAGING.Clustered_test2` 
   WHERE project_short_name = "TARGET-ALL-P2" AND CHROM = "chr1" AND POS = 161550724  AND ALT = "T"

