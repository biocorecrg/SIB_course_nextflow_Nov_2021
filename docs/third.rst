.. _second-page:

*******************
Third Day
*******************

During this day we will make more complex pipelines and separate the main code from the configuration. Then we will focus on the reuse of the code and on how to share your code.

.. image:: images/nextflow_logo_deep.png
  :width: 300
  

Decoupling resources, parameters and nextflow script
=======================

When you make a complex pipelines you might want to keep separated the definition of resources needed, the default parameters and the main script.
You can achieve this by two additional files:

- nextflow.config
- params.config

The **nextflow.config** file allows to indicate the resources needed for each class of processes.
You can label your processes to make a link with the definitions in the nextflow.config file. This is an example of a nextflow.config file:

.. code-block:: console

	includeConfig "$baseDir/params.config"

	process {
	     memory='0.6G'
	     cpus='1'
	     time='6h'

	     withLabel: 'onecpu'
		{
			memory='0.6G'
			cpus='1'
		} 	

	}

	process.container = 'biocorecrg/c4lwg-2018:latest'
	singularity.cacheDir = "$baseDir/singularity"


The first row indicates to use the information stored in the **params.config** file (described later). Then we have the definition of the default resources for a process:

.. code-block:: console
	process {
	     memory='0.6G'
	     cpus='1'
	     time='6h'
	...

Then we have the resources needed for a class of processes in particular labeled with **bigmem** (i.e. the default options will be overridden)

.. code-block:: console

	withLabel: 'bigmem'	
	{
		memory='0.7G'
		cpus='1'
	} 	

In the script **test2.nf file** we have two processes that will run two programs:
- `fastQC <https://www.bioinformatics.babraham.ac.uk/projects/fastqc/>`__: a tool that calculates a number of quality control metrics on single fastq files.
- `multiQC <https://multiqc.info/>`__: an aggregator of results from bioinformatics tools and samples for generating a single report.

If we have a look at process **fastQC** we can see the use of the label.

.. code-block:: groovy

	/*
	 * Process 1. Run FastQC on raw data.
	*/
	process fastQC {

	    publishDir fastqcOutputFolder  		
	    tag { "${reads}" }  					
	    label 'bigmem'

	    input:
	    path reads   							
	...


The last two rows of the config file indicate which container needs to be used. 
In this example, it is pulling it from `DockerHub <https://hub.docker.com/>__. 
In case you want to use a singularity container, you can indicate where to store the local image by using the **singularity.cacheDir** option.

.. code-block:: groovy

	process.container = 'biocorecrg/c4lwg-2018:latest'
	singularity.cacheDir = "$baseDir/singularity"


Let's now launch the script **test2.nf**.

.. code-block:: console

	cd test2;
	nextflow run test2.nf

	N E X T F L O W  ~  version 20.07.1
	Launching `test2.nf` [distracted_edison] - revision: e3a80b15a2
	BIOCORE@CRG - N F TESTPIPE  ~  version 1.0
	=============================================
	reads                           : /home/ec2-user/git/CoursesCRG_Containers_Nextflow_May_2021/nextflow/nextflow/test2/../testdata/*.fastq.gz
	executor >  local (2)
	[df/2c45f2] process > fastQC (B7_input_s_chr19.fastq.gz) [  0%] 0 of 2
	[-        ] process > multiQC                            -
	Error executing process > 'fastQC (B7_H3K4me1_s_chr19.fastq.gz)'

	Caused by:
	  Process `fastQC (B7_H3K4me1_s_chr19.fastq.gz)` terminated with an error exit status (127)

	Command executed:

	  fastqc B7_H3K4me1_s_chr19.fastq.gz

	Command exit status:
	  127

	executor >  local (2)
	[df/2c45f2] process > fastQC (B7_input_s_chr19.fastq.gz) [100%] 2 of 2, failed: 2 ✘
	[-        ] process > multiQC                            -
	Error executing process > 'fastQC (B7_H3K4me1_s_chr19.fastq.gz)'

	Caused by:
	  Process `fastQC (B7_H3K4me1_s_chr19.fastq.gz)` terminated with an error exit status (127)

	Command executed:

	  fastqc B7_H3K4me1_s_chr19.fastq.gz

	Command exit status:
	  127

	Command output:
	  (empty)

	Command error:
	  .command.sh: line 2: fastqc: command not found

	Work dir:
	  /home/ec2-user/git/CoursesCRG_Containers_Nextflow_May_2021/nextflow/nextflow/test2/work/c5/18e76b2e6ffd64aac2b52e69bedef3

	Tip: when you have fixed the problem you can continue the execution adding the option `-resume` to the run command line


We will get a number of errors since no executable is found in our environment / path. This because they are stored in our docker image! So we can launch it this time with the `-with-docker` parameter.


.. code-block:: console

	nextflow run test2.nf -with-docker

	nextflow run test2.nf -with-docker
	N E X T F L O W  ~  version 20.07.1
	Launching `test2.nf` [boring_hamilton] - revision: e3a80b15a2
	BIOCORE@CRG - N F TESTPIPE  ~  version 1.0
	=============================================
	reads                           : /home/ec2-user/git/CoursesCRG_Containers_Nextflow_May_2021/nextflow/nextflow/test2/../testdata/*.fastq.gz
	executor >  local (3)
	[22/b437be] process > fastQC (B7_H3K4me1_s_chr19.fastq.gz) [100%] 2 of 2 ✔
	[1a/cfe63b] process > multiQC                              [  0%] 0 of 1
	executor >  local (3)
	[22/b437be] process > fastQC (B7_H3K4me1_s_chr19.fastq.gz) [100%] 2 of 2 ✔
	[1a/cfe63b] process > multiQC                              [100%] 1 of 1 ✔


This time it worked beautifully since Nextflow used the image indicated within the nextflow.config file that contains our executables.

Now we can have a look at the **params.config** file

.. code-block:: console

	params {
		reads		= "$baseDir/../testdata/*.fastq.gz"
		email		= "myemail@google.com"
	}


As you can see we indicates the pipeline parameters that can be overridden by using `--reads` and `--email`.
This is not mandatory but I found quite useful to modify this file instead of using very long command lines with tons of `--something`.

Now, let's have a look at the folders generated by the pipeline.

.. code-block:: console

	ls  work/2a/22e3df887b1b5ac8af4f9cd0d88ac5/

	total 0
	drwxrwxr-x 3 ec2-user ec2-user  26 Apr 23 13:52 .
	drwxr-xr-x 2 root     root     136 Apr 23 13:51 multiqc_data
	drwxrwxr-x 3 ec2-user ec2-user  44 Apr 23 13:51 ..


We observe that Docker runs as "root". This can be problematic and generates security issues. To avoid this we can add this line of code within the process section of the config file:

.. code-block:: console

	containerOptions = { workflow.containerEngine == "docker" ? '-u $(id -u):$(id -g)': null}


This will tell Nextflow that if is running with Docker, this has to produce files that belong to your user and not to root.

Publishing final results
----------------------------

After running the script you see two new folders named **output_fastqc** and **output_multiQC** that contain the result of the pipeline.
We can indicate which process and which output can be considered the final output of the pipeline by using the **publishDir** directive that has to be specified at the beginning of a process.

In our pipeline we define these folders here:

.. code-block:: groovy

	/*
 	 * Defining the output folders.
 	 */
	
	fastqcOutputFolder    = "output_fastqc"
	multiqcOutputFolder   = "output_multiQC"

	[...]

	/*
	 * Process 1. Run FastQC on raw data. A process is the element for executing scripts / programs etc.
	 */
	 
	process fastQC {
	    publishDir fastqcOutputFolder  			// where (and whether) to publish the results

	[...]

	/*
	 * Process 2. Run multiQC on fastQC results
	 */
	 
	process multiQC {
	    publishDir multiqcOutputFolder, mode: 'copy' 	// this time do not link but copy the output file


You can see that the default mode to publish the results in Nextflow is soft linking. You can change this behaviour by specifying the mode as indicated in the **multiQC** process.

**IMPORTANT: You can also "move" the results but this is not suggested for files that will be needed for other processes. This will likely disrupt your pipeline.**

We can copy the output files to our `S3 bucket <https://docs.aws.amazon.com/AmazonS3/latest/userguide/UsingBucket.html>`__ to be accessed via web. Your bucket is mounted in **/mnt** 

.. code-block:: groovy

	ls /mnt

	/mnt/class-bucket-1



Your number can be different (i.e. class-bucket-2, class-bucket-3, etc) since we have one bucket per student. Let's copy the **multiqc_report.html** file there and let's change the privileges.

.. code-block:: console

	cp output_multiQC/multiqc_report.html /mnt/class-bucket-1

	sudo chmod 775 /mnt/class-bucket-1/multiqc_report.html 


Now you can see via browser at at:

.. code-block:: groovy
	http://class-bucket-1.s3.eu-central-1.amazonaws.com/multiqc_report.html


Of course again we need to change **class-bucket-1** with your own number.


Adding a help section for the whole pipeline
=============================================

In this example we also describe another good practice: the use of the `--help` parameter. At the beginning of the pipeline we can write:

.. code-block:: groovy

	params.help             = false    // this prevents a warning of undefined parameter

	// this prints the input parameters
	log.info """
	BIOCORE@CRG - N F TESTPIPE  ~  version ${version}
	=============================================
	reads                           : ${params.reads}
	"""

	// this prints the help in case you use --help parameter in the command line and it stops the pipeline
	if (params.help) {
	    log.info 'This is the Biocore\'s NF test pipeline'
	    log.info 'Enjoy!'
	    log.info '\n'
	    exit 1
	}

so launching the pipeline with `--help` will show you just the parameters and the help.

.. code-block:: groovy

	nextflow run test2.nf --help

	N E X T F L O W  ~  version 20.07.1
	Launching `test2.nf` [mad_elion] - revision: e3a80b15a2
	BIOCORE@CRG - N F TESTPIPE  ~  version 1.0
	=============================================
	reads                           : /home/ec2-user/git/CoursesCRG_Containers_Nextflow_May_2021/nextflow/nextflow/test2/../testdata/*.fastq.gz
	This is the Biocore's NF test pipeline
	Enjoy!

EXERCISE 
------------------

- Look at previous EXERCISE. Can you make a configuration for that script with a new label for handling failing processes? 

.. raw:: html

   <details>
   <summary><a>Solution</a></summary>

The process should become:

.. code-block:: groovy

	process reverseSequence {

	    tag { "${seq}" }                  
	    publishDir "output"
	    label 'ignorefail'

	    input:
	    path seq

	    output:
	    path "all.rev"

	    script:
	    """
	    cat ${seq} | AAAAA '{if (\$1~">") {print \$0} else system("echo " \$0 " |rev")}' > all.rev
	    """
	}
	

while the nextflow.config file would be:

.. code-block:: groovy

	process {
		withLabel: 'ignorefail'
		{
			errorStrategy = 'ignore' 
	    	}   	
	}

   
.. raw:: html

	</details>

- Now look at **test2.nf**.
Can you make a configuration for that script with a new label for handling failing processes by retrying 3 times and incrementing the time?

You can give very low time (10 / 15 seconds) for the fastqc process so it would fail at beginning. 

.. raw:: html

   <details>
   <summary><a>Solution</a></summary>


The process should become:

.. code-block:: groovy

	process fastQC {

		publishDir fastqcOutputFolder	// where (and whether) to publish the results
		tag { "${reads}" } 	// during the execution prints the indicated variable for follow-up
		label 'keep_trying' 

		input:
		path reads   	// it defines the input of the process. It sets values from a channel

		output:			// It defines the output of the process (i.e. files) and send to a new channel
   		path "*_fastqc.*"

    		script:			// here you have the execution of the script / program. Basically is the command line
    		"""
        	fastqc ${reads} 
   		"""
	}


while the nextflow.config file would be:

.. code-block:: groovy
	
	includeConfig "$baseDir/params.config"

 
	process {
	     //containerOptions = { workflow.containerEngine == "docker" ? '-u $(id -u):$(id -g)': null}
	     memory='0.6G'
	     cpus='1'
	     time='6h'

	     withLabel: 'keep_trying'	
		{ 
			time = { 10.second * task.attempt }
		errorStrategy = 'retry' 
		maxRetries = 3	
	    } 	

	}

	process.container = 'biocorecrg/c4lwg-2018:latest'
	singularity.cacheDir = "$baseDir/singularity"

.. raw:: html
	</details>


