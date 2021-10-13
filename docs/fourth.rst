.. _fourth-page:

*******************
Fourth Day
*******************

Introduction to Docker and Singularity containers.
============
TONI

Docker hub, BioContainers and other repositories. Find existing containers. Execute a Singularity container.
============
TONI

Introduction to Nextflow
============
A DSL for data-driven computational pipelines. `www.nextflow.io <https://www.nextflow.io>`_.

.. image:: images/nextflow_logo_deep.png
  :width: 400
  

What is Nextflow?
----------------

.. image:: images/nextf_groovy.png
  :width: 600

`Nextflow <https://www.nextflow.io>`__ is a domain specific language for workflow orchestration that stems from `Groovy <https://groovy-lang.org/>`__. It enables scalable and reproducible workflows using software containers. 
It was developed at the `CRG <www.crg.eu>`__ in the Lab of Cedric Notredame by `Paolo Di Tommaso <https://github.com/pditommaso>`__.
The Nextflow documentation is `available here <https://www.nextflow.io/docs/latest/>`__ and you can ask help to the community using their `gitter channel <https://gitter.im/nextflow-io/nextflow>`__

Nextflow has been upgraded in 2020 from DSL1 (Domain-Specific Language) version to DSL2. In this course we will use exclusively DSL2.  

What is Nextflow for?
----------------

It is for making pipelines without caring about parallelization, dependencies, intermediate file names, data structures, handling exceptions, resuming executions etc. 

It was published in `Nature Biotechnology in 2017 <https://pubmed.ncbi.nlm.nih.gov/28398311/>`__.

.. image:: images/NF_pub.png
  :width: 600
  

There is a growing number of publications mentioning Nextflow in `PubMed <https://pubmed.ncbi.nlm.nih.gov/?term=nextflow&timeline=expanded&sort=pubdate&sort_order=asc>`__, since many bioinformaticians are starting to write their pipeline with Nextflow.

.. image:: images/NF_mentioning.png
  :width: 600


Here is a curated list of `Nextflow pipelines <https://github.com/nextflow-io/awesome-nextflow>`__.

And here is a group of pipelines written in a collaborative way from the `NF-core <https://nf-co.re/pipelines>`__ project.

Some pipelines written in Nextflow are used for SARS-Cov-2 analysis, for example:

- the `artic Network <https://artic.network/ncov-2019>`__ pipeline: `ncov2019-artic-nf <https://github.com/connor-lab/ncov2019-artic-nf>`__.
- the `CRG / EGA viral Beacon <https://covid19beacon.crg.eu/info>`__ pipeline: `Master of Pores <https://github.com/biocorecrg/master_of_pores>`__.
- the nf-core pipeline: `viralrecon <https://nf-co.re/viralrecon>`__.


Main advantages
============

- **Fast prototyping**

You can quickly write a small pipeline that can be **expanded incrementally**.
**Each task is independent** and can be easily added to other ones.You can reuse your scripts and tools without rewriting / adapting them.

- **Reproducibility**

Nextflow supports **Docker and Singularity** containers technology. Their use will make the pipelines reproducible in any Unix environment. <br>Nextflow is integrated with **GitHub code sharing platform**, so you can call directly a specific version of pipeline from a repository, download it and use it on the fly.

- **Portability**

Nextflow can be executed on **multiple platforms** without modifiying the code. It supports several schedulers such as **SGE, LSF, SLURM, PBS and HTCondor** and cloud platforms like **Kubernetes, Amazon AWS and Google Cloud**.


.. image:: images/executors.png
  :width: 600

- **Scalability**

Nextflow is based on the **dataflow programming model** which simplifies writing complex pipelines.
The tool takes care of **parallelizing the processes** without additional written code.
The resulting applications are inherently parallel and can scale-up or scale-out, transparently, without having to adapt to a specific platform architecture.

- **Resumable, thanks to continuous checkpoints**

All the intermediate results produced during the pipeline execution are automatically tracked.
For each process **a temporary folder is created and is cached (or not) once resuming an execution**.

Workflow structure
============

The workflows can be represented as graphs where the nodes are the **processes** and the edges are the **channels**.
The **processes** are blocks of code that can be executed - such as scripts or programs - while the **channels** are asynchronous queues able to **connect processes among them via input / output**.


.. image:: images/wf_example.png
  :width: 600
  

Processes are independent from one another and can be run in parallel depending on the number of elements in a channel.
In the previous example, processes **A**, **B** and **C** can be run in parallel and only when they **ALL** end can process **D** be triggered.

Installation
============

.. note::
  Nextflow is already installed on the machines for the training!
  You need at least the Java version 8 for Nextflow installation. 

.. tip::
  You can check the version fo java by typing::

    java -version

Then we can install Nextflow with::

  curl -s https://get.nextflow.io | bash

This will create the ``nextflow`` executable that can be moved, for example, to ``/usr/local/bin``.

We can test that the installation was successful with:

.. code-block:: console

  nextflow run hello

  N E X T F L O W  ~  version 20.07.1
  Pulling nextflow-io/hello ...
  downloaded from https://github.com/nextflow-io/hello.git
  Launching `nextflow-io/hello` [peaceful_brahmagupta] - revision: 96eb04d6a4 [master]
  executor >  local (4)
  [d7/d053b5] process > sayHello (4) [100%] 4 of 4 ✔
  Ciao world!
  Bonjour world!
  Hello world!
  Hola world!


This command downloads and runs the pipeline ``hello``. It is downloaded from the `nextflow-io` `github repository <https://github.com/nextflow-io>`__.

ADD SOMETHING?

