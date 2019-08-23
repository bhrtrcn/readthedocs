*******************************
PanCancer Atlas BigQuery Mirror
*******************************

`The PanCancer BigQuery Mirror <https://bigquery.cloud.google.com/queries/pancancer-atlas>`_ -- produced in
collaboration with the `TCGA research network <https://cancergenome.nih.gov/>`_,
the `GDC <https://gdc.cancer.gov/>`_, and the `NCI <https://www.cancer.gov/>`_ -- provide
a new way to explore and understand the processes driving cancer.
The availability of PanCaner Atlas data in BigQuery enables easy integration of this
resource with other public datasets in BigQuery, including other
open-access datasets made available by the ISB-CGC
(see `this <http://isb-cancer-genomics-cloud.readthedocs.io/en/latest/sections/data/data2/data_in_BQ.html>`_
and `that <http://isb-cancer-genomics-cloud.readthedocs.io/en/latest/sections/data/Reference-Data.html>`_
for more details on other publicly accessible BigQuery datasets).

README
######

The Google BigQuery tables (`here <https://bigquery.cloud.google.com/queries/pancancer-atlas>`_) mirror the files shared by the PanCancer Atlas initiative on the `GDC <https://gdc.cancer.gov/about-data/publications/pancanatlas>`_.

For examples of usage, see `our blog <http://isb-cancer-genomics-cloud.readthedocs.io/en/latest/sections/QueryOfTheMonthClub.html>`_.

Use of the tables in the Filtered dataset is suggested.

The Staging dataset are essentially unmodified uploads of the file data.  The Staging tables are universally annotated as appropriate with ParticipantBarcode, SampleBarcode, AliquotBarcode, SampleTypeLetterCode, SampleType and TCGA Study, and put in Annotated. Then the Annotated tables are filtered using the PanCancer Atlas whitelist. Those filtered tables are found in the Filtered dataset.

An exception is the (public)  *MC3 MAF file*, which is found in the  Annotated dataset.


Getting Started
###############

`Register a cloud project <https://cloud.google.com/resource-manager/docs/creating-managing-projects>`_ for access to `BigQuery <https://cloud.google.com/bigquery/what-is-bigquery>`_ .

Adding the PanCancer Atlas tables to your workspace
###################################################

To add public BigQuery datasets and tables to your "view" in the `BigQuery web UI <https://bigquery.cloud.google.com/queries/pancancer-atlas>`_ you
need to know the name of the GCP project that owns the dataset(s).
To add the publicly accessible ISB-CGC datasets (project name: ``pancancer-atlas`` and ``isb-cgc``)
follow these steps_.

.. _steps: http://isb-cancer-genomics-cloud.readthedocs.io/en/latest/sections/progapi/bigqueryGUI/LinkingBigQueryToIsb-cgcProject.html

You should now be able to see and explore all of the PanCancer and ISB-CGC public datasets.
Clicking on the blue triangle next to a dataset name will open it and
show the list of tables in the dataset. Clicking on a table name will open up
information about the table in main panel, where you can
view the Schema, Details, or a Preview of the table.

Additional projects with public BigQuery datasets which you may want to explore (repeating
the same process will add these to your BigQuery side-panel) include genomics-public-data and
google.com:biggene.

Consider the ``isb-cgc:genomic_reference.SwissProt`` table;
a complete BigQuery table name has three components:

   * the first part of the name (isb-cgc) is the Google Cloud Platform (GCP) project name;
   * the second part (genomic_reference) is the dataset name; and
   * the third part (SwissProt) is the table name.


Troubleshooting
###############

After going through the registration process described above, there will be a short
delay before your Google identity is granted the necessary access to BigQuery and the PanCancer Atlas
data resources.  If you get an error when running the sample query in this section, please
wait 10-15 minutes and then try again. If you are still not successful, please
`verify <https://accounts.google.com/ForgotPasswd>`_
that the Google ID you have provided is a valid Google account.  If you are still not able
to run the sample query given below, please contact us at feedback@isb-cgc.org.


Interactive Web-based Exploration
#################################

Ready to query? Great! First see the pancancer-atlas:README, then follow the steps below to run your first BigQuery!

*Please note that some of the screen-shots on this page may be based on earlier versions of the PanCancer Atlas tables, but the sample SQL on this page has been updated (and tested) to query the latest PanCancer Atlas tables.*

* `login <https://accounts.google.com/Login>`_ to your Google account (`Chrome <https://www.google.com/chrome/browser/desktop/index.html>`_ is the preferred browser);
* go to the `BigQuery web UI <https://bigquery.cloud.google.com>`_  --  if you see a welcome screen inviting you to **Create a Project** then please do so;

Let's query using the MC3 somatic mutation table.

* click on the big red **COMPOSE QUERY** button in the upper left corner;
* click on the **Show Options**  button below the **New Query** text-box;
* un-check the **Use Legacy SQL** check-box (the bottom-most "option");
* click on the **Hide Options** button;
* paste the sample query below into the New Query text-box;
* within a second or two you should see a green circle with a check-mark below the lower-right-corner of the New Query text-box  --  if instead you see a red circle with an exclamation mark, click on it to see what your Syntax Error is;
* once you do have the green circle, you can click on it to see a message like: "Valid: This query will process 131 MB when run."
* to execute the query, click on **RUN QUERY** !


.. code-block:: sql

    WITH
      mutCounts AS (
      SELECT
        COUNT(DISTINCT(sample_barcode_tumor)) AS CaseCount,
        Hugo_Symbol,
        HGVSc
      FROM
        `pancancer-atlas.Annotated.mc3_v0_2_8_PUBLIC_maf`
      GROUP BY
        Hugo_Symbol,
        HGVSc
      ),
      mutRatios AS (
      SELECT
        HGVSc,
        Hugo_Symbol,
        CaseCount,
        (CaseCount/SUM(CaseCount) OVER (PARTITION BY Hugo_Symbol)) AS ratio
      FROM
        mutCounts )
    SELECT
      *
    FROM
      mutRatios
    WHERE
      CaseCount>=10
      AND ratio>=0.2
      AND HGVSc is not null
    ORDER BY
      ratio DESC


Additional BigQuery Documentation
#################################

The main Google BigQuery documentation can be found `here <https://cloud.google.com/bigquery/docs/>`_.

Legacy SQL vs Standard SQL
--------------------------

BigQuery introduced support for
`Standard SQL <https://cloud.google.com/bigquery/docs/reference/standard-sql/>`_
in 2016.  The previous version of SQL supported by
BigQuery is now known as
`Legacy SQL <https://cloud.google.com/bigquery/docs/reference/legacy-sql>`_.
Note that when you first go to the BigQuery web UI,
Legacy SQL will be activated by default and you will need to enable Standard SQL if you want to
use Standard SQL.  For simple queries, the same syntax will work in both, except for one
important detail which is how you specify the table name.  A simple Standard SQL query might look like:

.. code-block:: sql

    SELECT *
      FROM `isb-cgc.TCGA_hg38_data_v0.Somatic_Mutation_DR10`
      LIMIT 1000

whereas the same query in Legacy SQL requires square brackets around the table name and a colon
between the project name and the dataset name, like this:

.. code-block:: sql

    SELECT *
      FROM [isb-cgc:TCGA_hg38_data_v0.Somatic_Mutation_DR10]
      LIMIT 1000

(Although please note that you can use the "Preview" feature in the BigQuery web UI, at no cost, instead of doing a SELECT * which will do a full table scan!)

SQL functions
-------------

Standard SQL includes a large variety of built-in
`functions and operators <https://cloud.google.com/bigquery/docs/reference/standard-sql/functions-and-operators>`_
including logical and statistical aggregate functions, and mathematical functions, just to name a few.
`User-defined functions <https://cloud.google.com/bigquery/docs/reference/standard-sql/user-defined-functions>`_ (UDFs)
are also supported and can be used to further extend the types of analyses possible in BigQuery.

Using the bq Command Line Tool
------------------------------
The **bq** command line tool is part of the
`cloud SDK <https://cloud.google.com/sdk/>`_ and can be used to interact directly
with BigQuery from the command line.  The cloud SDK is easy to install and
is available for most operating systems.  You can use **bq** to create and upload
your own tables into BigQuery (if you have your own GCP project),
and you can run queries at the command-line like this:

.. code-block:: none

   bq query --allow_large_results \
            --destination_table="myproj:dataset:query_output" \
            --nouse_legacy_sql \
            --nodry_run \
            "$(cat myQuery.sql)"

(where myQuery.sql is a plain-text file containing the SQL, and the destination
table is in an existing BigQuery dataset in your project).

Using BigQuery from R
---------------------
BigQuery can be accessed from R using one of two powerful R packages:
`bigrquery <https://cran.r-project.org/web/packages/bigrquery/>`_ and
`dplyr <https://cran.r-project.org/web/packages/dplyr/>`_.
Please refer to the documentation provided with these packages for more information.

Using BigQuery from Python
--------------------------
BigQuery
`client libraries <https://cloud.google.com/bigquery/docs/reference/libraries#client-libraries-install-python>`_
are available that let you interact with BigQuery from Python or other languages.
In addition, the experimental
`pandas.io.gbq <http://pandas.pydata.org/pandas-docs/stable/io.html#google-bigquery-experimental>`_
module provides a wrapper for BigQuery.

Getting Help
------------
Aside from the documentation, the best place to look for help using BigQuery and tips
and tricks with SQL is
`StackOverflow <http://stackoverflow.com/>`_.  If you tag your question with ``google-bigquery``
your question will quickly get the attention of Google BigQuery experts.  You may also find
that your question has already been asked and answered among the nearly 10,000 questions
that have already been asked about BigQuery on StackOverflow.

More SQL Examples
#################

Let's start with a few simple examples to get some practice using BigQuery, and to
explore some of the available fields in these PanCancer Atlas tables.

Note that all of these examples are in "Standard SQL", so make sure that you have that enabled.
(See instructions above regarding un-checking the "Legacy SQL" box in the BigQuery web UI.)

**1. How many mutations have been observed in KRAS?**

.. code-block:: sql

    SELECT
      COUNT(DISTINCT(sample_barcode_tumor)) AS numSamples
    FROM
      `isb-cgc.TCGA_hg38_data_v0.Somatic_Mutation_DR10`
    WHERE
      Hugo_Symbol="KRAS"

You can simply copy-and-paste any of the SQL queries on this page into the
`BigQuery web UI  <https://bigquery.cloud.google.com>`_ .  The screen-shot
shown here shows the query in the "New Query" box, and the results
down below.  Just click on the "RUN QUERY" button to run the query.
Notice the green check-mark indicating that the query looks good.


**2. What other information is available about these KRAS mutant tumours?**

In addition to answering the question above,
this next query also illustrates usage of the **WITH** construct to create an intermediate
table on the fly, and then use it in a follow-up **SELECT**:

.. code-block:: sql

    WITH
      t1 AS (
      SELECT
        project_short_name,
        sample_barcode_tumor,
        Hugo_Symbol,
        Variant_Classification,
        Variant_Type,
        SIFT,
        PolyPhen
      FROM
        `isb-cgc.TCGA_hg38_data_v0.Somatic_Mutation_DR10`
      WHERE
        Hugo_Symbol="KRAS"
      GROUP BY
        project_short_name,
        sample_barcode_tumor,
        Hugo_Symbol,
        Variant_Classification,
        Variant_Type,
        SIFT,
        PolyPhen )
    SELECT
      COUNT(*) AS n,
      Hugo_Symbol,
      Variant_Classification,
      Variant_Type,
      SIFT,
      PolyPhen
    FROM
      t1
    GROUP BY
      Hugo_Symbol,
      Variant_Classification,
      Variant_Type,
      SIFT,
      PolyPhen
    ORDER BY
      n DESC

**3. What are the most frequently observed mutations and how often do they occur?**

.. code-block:: sql

    WITH
      t1 AS (
      SELECT
        sample_barcode_tumor,
        Hugo_Symbol,
        Variant_Classification,
        Variant_Type,
        SIFT,
        PolyPhen
      FROM
        `isb-cgc.TCGA_hg38_data_v0.Somatic_Mutation_DR10`
      GROUP BY
        sample_barcode_tumor,
        Hugo_Symbol,
        Variant_Classification,
        Variant_Type,
        SIFT,
        PolyPhen )
    SELECT
      COUNT(*) AS n,
      Hugo_Symbol,
      Variant_Classification,
      Variant_Type,
      SIFT,
      PolyPhen
    FROM
      t1
    GROUP BY
      Hugo_Symbol,
      Variant_Classification,
      Variant_Type,
      SIFT,
      PolyPhen
    ORDER BY
      n DESC


**Stay-tuned, more examples coming soon!**

If you have a specific use-case that you need help with, feel free to contact us!
