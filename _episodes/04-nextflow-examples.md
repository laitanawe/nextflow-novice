---
title: "Nextflow Examples"
teaching: 10
exercises: 50
questions:
- "What are the main sections of a Nextflow process?"
- "What are the main components of a Nextflow pipeline?"
- "How do I run a Nextflow pipeline?"
objectives:
- "Explain the benefits of using Nextflow to develop pipelines."
- "Explain the components of a Nextflow process, pipeline."
- "Run a Nextflow pipeline."
keypoints:
- "A workflow is a sequence of tasks that process a set of data."
- "Nextflow scripts comprise of channels for controlling inputs and outputs, and processes for defining workflow tasks."
- "You run Nextflow modules and pipelines using the `nextflow run` command."
---

## Our example script

We are now going to look at sample Nextflow scripts.

~~~
#!/usr/bin/env nextflow

/* Contributors:
 * - Awe, O. (laitanawe@gmail.com)
 */

nextflow.enable.dsl=2

/*
 * Default pipeline parameters can be overriden on the command line eg.
 * given 'params.foo' specify on the run command line '--foo some_value'
 */

 process concat {

 params.readpattern = "${baseDir}/data/ggal/*_1.fq"
 params.reads = Channel.fromPath(params.readpattern)
 params.concatout = "concatenated_fwd.txt"
 params.twentyout = "first_twenty_fwd.txt"

 input:
 path(myreads) from params.reads.collect()

 script:
 """
 cat ${myreads} > ${params.concatout}
 head -20 ${params.concatout} > ${params.twentyout}
 """

 }
~~~~
{: .language-groovy}

To run a Nextflow script use the command `nextflow run <script_name>`.

> ## Run a Nextflow  script
> Run the script (ex623.nf) by entering the following command(s) in your terminal:
>
> ~~~
> $ nextflow run ex623.nf -process.echo
> $ nextflow run ex623.nf --readpattern 'data/ggal/*_2.fq' --concatout 'concatenated_rev.txt' --twentyout 'first_twenty_rev.txt' -process.echo
> ~~~
> {: .language-bash}
> > ## Solution
> > You should see output similar to the text shown below:
> >
> > ~~~
> > N E X T F L O W  ~  version 20.10.0
> > Launching `main.nf` [fervent_babbage] - revision: c54a707593
> > executor >  local (1)
> > [21/b259be] process > NUM_LINES (1) [100%] 1 of 1 ✔
> >
> >  ref1_1.fq.gz 58708
> > ~~~
> > {: .output}
> >
> > 1. The first line shows the Nextflow version number.
> > 1. The second line shows the run name `fervent_babbage` (adjective and scientist name) and revision id `c54a707593`.
> > 1. The third line tells you the process has been executed locally (`executor >  local`).
> > 1. The next line shows the process id `21/b259be`, process name, number of cpus, percentage task completion, and how many instances of the process have been run.
> > 1. The final line is the output of the `view` operator.
> {: .solution}
{: .challenge}


> ## Process identification
> The hexadecimal numbers, like 61/1f3ef4, identify the unique process execution.
> These numbers are also the prefix of the directories where each process is executed.
> You can inspect the files produced by changing to the directory `$PWD/work` and
> using these numbers to find the process-specific execution path. We will learn how exactly
> nextflow using *work* directory to execute processes in the following sections.
{: .callout}



{% include links.md %}
