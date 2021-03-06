
.. Non-breaking white space, to fill empty divs
.. |nbsp| unicode:: 0xA0
   :trim:

.. role:: strike
    :class: strike

About Varapp
============

What is Varapp ?
----------------

Varapp is a web application to filter genetic variants, with a reactive graphical user interface.

The application has been developed in a collaboration between the
Swiss Institute of Bioinformatics (SIB) and the Lausanne University Hospital (CHUV).

It is meant to be deployed as a private web service for a research group.

.. figure:: /images/main-border.png
   :width:  100%
   :alt: Main window


Motivation
..........

Common filtering pipelines can be very tedious, including an annotation step,
complicated forms, multiple exports to spreadsheets, merging and manual curation
of theses spreadsheets.
Although there already exist several tools to facilitate the annotation of lists of variants
and apply basic filters such as frequency in a population, quality or location,
Varapp has at least one of the following advantages over existing software:

*   **Security**: It does not require to upload data to a cloud or external client;
    one can deploy the app on any local computer and keep the data private.

*   **Fast, non-trivial filtering** based on family pedigree, including complex cases
    such as compound heterozygous, in less than a second for 500K variants.

*   **Reproducibility**: It uses a relational database, adding the potential to cross information
    between experiments, reuse the results of previously studied data,
    and keep track of the annotation/versions used to produce the results.

*   **Reactive user interface**: Apply a filter and see the result immediately.

*   **Easy sharing** of the results: send a URL to a colleague, and he sees
    the current state of your analysis just as you do.

Use cases
.........

Varapp is typically well-suited to cover the following two use cases,
the result of which is obtained within a few seconds:

* **Mendelian diseases**

    We consider all variants of one family and search for the variants that are
    susceptible to cause an hereditary disease. Varapp provides one-click filters
    for dominant, recessive, de novo, X-linked, and compound heterozygous cases.

* **Gene panels**

    We consider the same few genes (typically a few hundred)
    in a large series of independent individuals. We seek rare variants with
    strong impact, then inspect the individual(s) that carry them.

Varapp is not...
................

* a variants caller:

    It starts after the variant calling, and uses multi-sample VCF files
    generated by variant callers (e.g. GATK).

* a variants annotation tool:

    We rely on `Gemini <https://gemini.readthedocs.org/en/latest/>`_
    and `VEP <http://www.ensembl.org/info/docs/tools/vep/index.html>`_
    to annotate the input VCF. No data is added or modified afterwards.

* adapted to whole genome sequencing, or to thousands of samples:

    For the moment, the app is responsive with datasets up to 500???000 variants
    and a few hundred individuals. Performance issues are expected with larger datasets.
    For exome sequencing on large cohorts, we recommend to split the dataset in batches
    of 100-200 samples.


How can I try it?
-----------------

One public, demo version of the program is available at
`https://varapp-demo.vital-it.ch <https://varapp-demo.vital-it.ch>`_ .

.. note::

    This version runs or minimal hardware, thus its performance is not comparable
    to a real production setup.

Log in as user "demo" with password "demo".
You will be granted access to a variants database "demo".
Try the following standard filters:

- Recessive scenario
- 1% frequencies in 1000 Genomes, ESP and EXAC
- Quality filter: PASS (VQSR)
- HIGH/MED impact

You should retrieve a single variant on gene TBC1D7 that has been published
in `Hum Mutat. 2014 Apr;35(4):447-51 <http://www.ncbi.nlm.nih.gov/pubmed/24515783>`_.

.. note::

    Prior to making it public, this dataset has been anonymized and obfuscated.
    Parents variant data was generated by random shuffling variants from several
    independent exome datasets. Children variant data was derived from the
    shuffled parents data. The TBC1D7 pathogenic variant was then added
    following recessive pattern of inheritance.

A second, public dataset "hapmap" of 170K variants is available 
(source: `HapMap <http://sra.dnanexus.com/studies/SRP007298/runs>`_).
Switch to it using the database selection button in the top right corner. 
Try double-clicking a variant to view the read alignment that
is at the origin of the call. (To save disk space, only alignments 
on the first 3M bp of chromosome 1 are available).


Reference
---------

Please add this reference to your publication if Varapp helped you in your research: 
`<http://biorxiv.org/content/early/2016/06/27/060806>`_.

**doi**: http://dx.doi.org/10.1101/060806


Contributing
------------

The source code is available in `Github <https://github.com/varapp>`_.
Bug reports, suggestions, contributions and comments are all welcome.


Future plans
------------

These are the new features that we are either working on now
or would like to add progressively to the program.

- Get rid of VEP for the annotation.
- :strike:`View local alignments in a viewer similar to IGV.`
- :strike:`Docker support`.
- Scale up to full genome.
- More flexibility with the annotation.
- Calculate variants frequencies across several databases of one user/group.

