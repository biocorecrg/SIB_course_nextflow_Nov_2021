.. _first-page:

*******************
First Day
*******************

Introduction to Docker and Singularity containers.
============

What are containers?
---------------------

.. image:: https://www.synopsys.com/blogs/software-security/wp-content/uploads/2018/04/containers-rsa.jpg
  :width: 700

A Container can be seen as a **minimal virtual environment** that can be used in any Linux-compatible machine (and beyond).

Using containers is time- and resource-saving as they allow:

* Controlling for software installation and dependencies.
* Reproducibility of the analysis.

Containers allow us to use **exactly the same versions of the tools**.

Virtual machines or containers ?
----------------------------------

| Virtualisation | Containerisation (aka lightweight virtualisation) |
| ----- | ----- |
| Abstraction of physical hardware | Abstraction of application layer |
| Depends on hypervisor (software) | Depends on host kernel (OS) |
| Do not confuse with hardware emulator | Application and dependencies bundled all together |
| Enable virtual machines:<br>Every virtual machine with an OS (Operating System) ||

Virtualisation
----------------------------------

* Abstraction of physical hardware
* Depends on hypervisor (software)
* Do not confuse with hardware emulator
* Enable virtual machines:
	* Every virtual machine with an OS (Operating System)

Containerisation (aka lightweight virtualisation)
----------------------------------

* Abstraction of application layer
* Depends on host kernel (OS)
* Application and dependencies bundled all together

### Virtual machines vs containers

.. image:: https://raw.githubusercontent.com/collabnix/dockerlabs/master/beginners/docker/images/vm-docker5.png
  :width: 800

`Source <https://dockerlabs.collabnix.com/beginners/difference-docker-vm.html>`__



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
----------------


- **Fast prototyping**

You can quickly write a small pipeline that can be **expanded incrementally**.
**Each task is independent** and can be easily added to other ones. You can reuse your scripts and tools without rewriting / adapting them.

- **Reproducibility**

Nextflow supports **Docker and Singularity** containers technology. Their use will make the pipelines reproducible in any Unix environment. Nextflow is integrated with **GitHub code sharing platform**, so you can call directly a specific version of pipeline from a repository, download it and use it on the fly.

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


This command downloads and runs the pipeline ``hello``.

We can now launch a test pipeline to show what will be using a nextflow pipeline:

.. code-block:: console

  nextflow run nextflow-io/rnaseq-nf -with-singularity

The command will automatically pull the pipeline and the required test data from the `github repository <https://github.com/nextflow-io/rnatoy>`__
The command ``-with-singularity`` will trigger automatically the download of the image ``nextflow/rnatoy:1.3`` from DockerHub and convert it on the fly into a singularity image that will be used for running each step of the pipeline.
Moreover the pipeline can also recognize the kind of queue system used where is launched. In the following examples I launched the same pipeline both on the CRG high performance computing centre (HPC) and on my MacBook:

The result from CRG's HPC:

.. code-block:: console

	nextflow run nextflow-io/rnaseq-nf -with-singularity

	N E X T F L O W  ~  version 21.04.3
	Pulling nextflow-io/rnaseq-nf ...
	downloaded from https://github.com/nextflow-io/rnaseq-nf.git
	Launching `nextflow-io/rnaseq-nf` [serene_wing] - revision: 83bdb3199b [master]
	R N A S E Q - N F   P I P E L I N E
	 ===================================
	transcriptome: /users/bi/lcozzuto/.nextflow/assets/nextflow-io/rnaseq-nf/data/ggal/ggal_1_48850000_49020000.Ggal71.500bpflank.fa
	reads        : /users/bi/lcozzuto/.nextflow/assets/nextflow-io/rnaseq-nf/data/ggal/*_{1,2}.fq
	outdir       : results

	[-        ] process > RNASEQ:INDEX  -
	[-        ] process > RNASEQ:FASTQC -
	executor >  crg (6)
	[cc/dd76f0] process > RNASEQ:INDEX (ggal_1_48850000_49020000) [100%] 1 of 1 ✔
	[7d/7a96f2] process > RNASEQ:FASTQC (FASTQC on ggal_liver)    [100%] 2 of 2 ✔
	[ab/ac8558] process > RNASEQ:QUANT (ggal_gut)                 [100%] 2 of 2 ✔
	[a0/452d3f] process > MULTIQC                                 [100%] 1 of 1 ✔

	Pulling Singularity image docker://quay.io/nextflow/rnaseq-nf:v1.0 [cache /nfs/users2/bi/lcozzuto/aaa/work/singularity/quay.io-nextflow-rnaseq-nf-v1.0.img]
	WARN: Singularity cache directory has not been defined -- Remote image will be stored in the path: /nfs/users2/bi/lcozzuto/aaa/work/singularity -- Use env  variable NXF_SINGULARITY_CACHEDIR to specify a different location
		Done! Open the following report in your browser --> results/multiqc_report.html

	Completed at: 01-Oct-2021 12:01:50
	Duration    : 3m 57s
	CPU hours   : (a few seconds)
	Succeeded   : 6


The result from my MacBook:

.. code-block:: console

	nextflow run nextflow-io/rnaseq-nf -with-docker

	N E X T F L O W  ~  version 21.04.3
	Launching `nextflow-io/rnaseq-nf` [happy_torvalds] - revision: 83bdb3199b [master]
	R N A S E Q - N F   P I P E L I N E
	===================================
	transcriptome: /Users/lcozzuto/.nextflow/assets/nextflow-io/rnaseq-nf/data/ggal/ggal_1_48850000_49020000.Ggal71.500bpflank.fa
	reads        : /Users/lcozzuto/.nextflow/assets/nextflow-io/rnaseq-nf/data/ggal/*_{1,2}.fq
	outdir       : results

	executor >  local (6)
	[37/933971] process > RNASEQ:INDEX (ggal_1_48850000_49020000) [100%] 1 of 1 ✔
	[fe/b06693] process > RNASEQ:FASTQC (FASTQC on ggal_gut)      [100%] 2 of 2 ✔
	[73/84b898] process > RNASEQ:QUANT (ggal_gut)                 [100%] 2 of 2 ✔
	[f2/917905] process > MULTIQC                                 [100%] 1 of 1 ✔

	Done! Open the following report in your browser --> results/multiqc_report.html



This is just an example of the power of the automation of the Nextflow environment.
