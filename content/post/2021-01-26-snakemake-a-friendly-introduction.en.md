---
title: "Snakemake - A Friendly Introduction"
author: "Jakob Willforss"
date: '2021-01-26'
output: pdf_document
Description: ''
categories:
- dataprocessing
- tutorial
DisableComments: no
Tags: []
slug: snakemake-a-friendly-introduction
tags:
- beginner
- tutorial
- snakemake
Categories: []
---

# Introduction

A PDF version of this workshop can be downloaded from [here](PDF).

### Making your analysis reproducible

Snakemake is a workflow-based system built to make reproducible analyses easy to build, extend and rerun.

There are several arguments for using it in your bioinformatics analyses:

* Reproducibility - preserve the exact sequence of commands used to generate certain results.
* Modularity - giving it flexibility, and making it easy to follow the analysis flow.
* Ease of parallel processing.
* Support for managing versions in containers such as [Docker](https://www.docker.com) or [Singularity](https://sylabs.io).

I have also found notebook-software such as [Jupyter notebooks](https://jupyter.org/) or [R Notebook](https://blog.rstudio.com/2016/10/05/r-notebooks/) to complement Snakemake well when it comes to visualizations and statistical analysis.

Finally, I would recommend version-controlling your workflow and your scripts in a system such as [Git](https://git-scm.com). This allows you to later come back and see exactly how the code looked at an earlier stage. If interested, I have written a tutorial for bioinformaticians [here](https://www.jakobwillforss.com/post/introduction-to-git-for-bioinformaticians/).

Further discussion on how to nicely carry out bioinformatics analysis is available in the article [Organizing your bioinformatics projects](https://journals.plos.org/ploscompbiol/article?id=10.1371/journal.pcbi.1000424).

It can be argued how to best do a reproducible analysis. I think the most important aspect is to care about the reproducibility and flexibility of your analysis, and try out different strategies to achieve it. Over time you will find the set of tools which best suits your needs.

But with that said, I think you will find Snakemake a powerful addition in your set of analysis-tools, well worth the relatively limited time and effort needed to pick up.

# Getting started

### Installation

For Linux-users snakemake can easily be installed through your package system (`apt install snakemake` for Ubuntu-users, or Windows Linux subsystem users).

For Windows-users who don't use the [Linux subsystem](https://docs.microsoft.com/en-us/windows/wsl/install-win10), it is a little bit more tricky. The recommended way is to run it through Miniconda. You find the recommended steps to install Snakemake [here](https://snakemake.readthedocs.io/en/stable/tutorial/setup.html).

### The exercise

The aim is to layer-by-layer introduce the fundamental parts of Snakemake which covers most of the basic bioinformatics use-cases. In the end of this tutorial, you will be able to:

* Build a workflow which can take an arbitrary number of input files.
* Reprocess these in parallel, efficiently using the computer's resources.
* Merge these into a final output file.
* Generate visualizations for this output file.

All of this can subsequently be carried out in a single step, and easily be further extended.

Both the data and required scripts and snakefiles with solutions for each step are available at the [GitHub repository](https://github.com/Jakob37/SnakemakeAFriendlyIntroduction).

You can download the folder by pressing the green button "Code" and either choosing "Download ZIP" or cloning it (if you are familiar with Git).

I would encourage you to follow along the exercise by writing out your own "snakefile", located at the top level in this directory. If you get stuck, you can then always refer to the different solutions.

### The data

Subsets of the data are found in the GitHub repository. For the interested, the full-sized dataset can also be downloaded from [here](XXX).

The data is originally from a study investigating the gut microbiota of ants from different colonies (Benjamino1 and Graf, 2016). The study can be accessed [here](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC4756164).

We will investigate the GC-content from the microbiome found at different colonies. To do this, we will build a pipeline which can:

1. Reprocess the data from raw FASTQ-files into FASTA-files.
2. Extract key information from these reads.
3. Summarize this information into a single matrix.
4. Generate a first overview visualization of this matrix.

This workflow will contain most of the core functionalities you need to use Snakemake to automate your own bioinformatics workflows.

For further help, take a look at the [Snakemake FAQ](https://snakemake.readthedocs.io/en/stable/project_info/faq.html), or search the internet. Most likely many other have had the same issue!

# Step 1: The Snakemake 'rule'

The Snakemake workflow consists of rules. These rules have `inputs` and `outputs`, which specify what files are expected for them to run, and which files are expected to be generated by the rule.

In each rule, a piece of code is executed. This can be written out directly in the snakemake workflow (commonly using Python or Bash), or called as an external script. Here, I have prepared a collection of Python-scripts executed by the different Snakemake rules.

### A first snakemake rule

Here is a simple example of a snakemake rule. It reads the file `data/21AL.A_R1.fastq`, executes the Python-script `fastq_to_fasta.py`, which in turn is expected to generate output to `output/1_fasta/21AL.A_R1.fasta`. Beyond this, the `--max_entries` specifies how big subset of the FASTA we should use. During the development, let's use a small number of sequences.

The backslash '\\' syntax is used to break lines into multiple lines. If everything was on a single line, the backslashes would be omitted.

```
rule convert_fastq_to_fasta:
    input: "data/21AL.A_R1.fastq"
    output: "output/1_fasta/21AL.A_R1.fasta"
    shell:
        """
        python3 scripts/fastq_to_fasta.py \
          --input {input} \
          --output {output} \
          --max_entries 100
        """
```

**Place this rule in your `snakefile`. At this point, this will be the only code.**

### Running snakemake

Now let's execute our snakemake-script. We run snakemake workflows by using the `snakemake` command. When performed with no arguments, it will look for a file named `snakefile` or `Snakefile` in your working directory. 

The `$` sign is used in this tutorial to show the line where the command is executed. When running it, you only need to write out "snakemake".

```
$ snakemake
```

If you want to run the solutions provided in the `snakefile/` directory, they can be executed one and one using the `--snakefile` command, where you can state custom locations for the snakefile. Note that you need to be in the top-level dictionary for the paths to the input to be correct.

```
$ snakemake --snakefile snakefiles/1_single_rule_snakefile
```

If your current path isn't correct relative to the input files, you might encounter an error like the following. If so, make sure that you are next to the "data" folder, and that the file "21AL.A_R1.fastq" is one of the files in this folder.

```
snakemake
Building DAG of jobs...
MissingInputException in line 1 of /home/user/snakemake_workshop/snakefile:
Missing input files for rule convert_fastq_to_fasta:
"data/21AL.A_R1.fastq"
```

If run successfully, you will obtain output looking something like the following:

```
$ snakemake
Building DAG of jobs...
Using shell: /bin/bash
Provided cores: 12
Rules claiming more threads will be scaled down.
Job counts:
        count   jobs
        1       convert_fastq_to_fasta
        1

[Tue Jan 26 09:12:04 2021]
rule convert_fastq_to_fasta:
    input: data/21AL.A_R1.fastq
    output: output/1_fasta/21AL.A_R1.fasta
    jobid: 0

[Tue Jan 26 09:12:04 2021]
Finished job 0.
1 of 1 steps (100%) done
Complete log: .../210125_snakemake_workshop/.snakemake/log/2021-01-26T091204.673160.snakemake.log
```

We see that Snakemake had up to 12 cores to run in parallel for the job (`Provided cores: 12`). We also see that one job was executed, `convert_fastq_to_fasta`, which was only executed a single time.
We also obtained a path to the error log (`2021-01-26T091204.673160.snakemake.log`). Here, further output and helpful error messages could often be found.

If the command failed to run successfully, carefully inspect the errors obtained in the terminal or in the error log. Often, the error is related to some simple syntax mistake, or that the files are not where you expect them to be compared to the current working directory.

## Exercises

1. Try to run the `snakemake` command. Inspect the output, and see if you understand the different parts.
2. Inspect the "output" folder. What do you see there? Is it what you would expect?
3. Try running `snakemake` again. What happened? Snakemake can figure out what rules needs to be run to generate the desired output. If nothing has changed, it will not re-run any code if not prompted to.
4. Try running `snakemake` with the `-F` flag (`snakemake -F`). What happened now? The "-F" flag allows you to force-run the workflow, even though no files have changed.
5. Finally, try using the `-n` flag. This will give you a dry-run, which is useful to verify that there are no apparent issues with the snakemake workflow.

# Step 2: A multi-rule workflow and the configuration file

Now we will run a sequence of two commands - a mini pipeline. To do this,
we will need to add two things. Snakemake files generally include a special
rule at the top called `rule all`, where you tell Snakemake what final output
files you want to obtain are. Based on what you specify here, Snakemake will then deduce which of the other rules it needs to run to generate this output.

Furthermore, we will run a second command used to calculate some statistics for each FASTA-sequence, including the GC-content. This is of particular interest in this dataset, as we know that the GC-content vary between species. But does it vary between different microbiomes?

The second command takes the output from the first command as input (`output/1_fasta/21AL.A_R1.fasta`). It then is expected to produce the output file `output/2_summary/21AL.A_R1.tsv`. This is provided as the input to `rule all`, as this is the final provided output.

```
configfile: "config.yaml"

rule all:
    input: 'output/2_summary/21AL.A_R1.tsv'

rule convert_fastq_to_fasta:
    input: 'data/21AL.A_R1.tsv.fastq'
    output: 'output/1_fasta/21AL.A_R1.tsv.fasta'
    shell:
        """
        python3 scripts/fastq_to_fasta.py \
          --input {input} \
          --output {output} \
          --max_entries {config[settings][max_nbr_seqs]}
        """

rule retrieve_fasta_stats:
    input: 'output/1_fasta/21AL.A_R1.tsv.fasta'
    output: 'output/2_summary/21AL.A_R1.tsv.tsv'
    shell:
        """
        python3 scripts/retrieve_fasta_stats.py \
          --input {input} \
          --output {output}
        """
```

The sequence of events when executing this script will be:
1. Ask the rule all - which files do I need to generate?
Answer: `output/2_summary/21AL.A_R1.tsv`
2. Are there any commands that can prove this? Yes, rule `retrieve_fasta_stats`.
3. Is the input to this rule ('output/1_fasta/21AL.A_R1.tsv.fasta') available on disk? No,
then are there any rules that can generate it? Yes, rule `convert_fastq_to_fasta`.
So, we need to execute that one first.
4. Is the input file `data/21AL.A_R1.tsv.fastq` for this command? Yes, per-
fect, then we are all set.
5. Processing starts, running first rule `convert_fastq_to_fasta` and next rule `retrieve_fasta_stats`.

In addition, we included a reference to a configuration file "config.yaml". This file is located in the same folder as your snakefile, with the following content:

```
settings:
  max_nbr_seqs: 100
```

This setting is now provided to the first commands option `--max_entries`, which allows us to limit the number of FASTQ-entries we use. It is a good habit to have to separate out settings we might want to change to the configuration file such that we later can avoid doing changes directly in the snakefile when we only want to change parameter values.

## Exercises:

1. After running your mini-pipeline, try rerunning it. What happens?
2. Next, try to remove the intermediate output file `output/1_fasta/21AL.A_R1.tsv.fasta` and try to rerun. What happens?
3. The commands `snakemake --dag | dot -Tsvg > dag.svg` can be used to generate a graph which illustrates the different executed commands. Can you get it to run? Does the graph look like you expected? (It is trivial at this point, but will be more useful later in this exercise).
4. Try changing the `max_nbr_seqs` in the config file (for instance to 200), and verify that the number of entries in the final output changes.

Illustration of the workflow at this point (generated in exercise 3):

![Workflow illustration, step 2](/post/2021-01-26-snakemake-a-friendly-introduction.en_files/step2_dag.svg)

# Step 3 - Multiple input files

Now, you have seen how we can run multiple rules for one input file. More commonly, we have multiple input files. Let's calculate these stats for all the available files.

Remember that Snakemake is written in Python, so you are free to use common Python code throughout the Snakemake scripts. The most straight-forward way to process multiple files is to include them in a Python list, and then use the command `expand` to tell the Snakemake rules to look for all these file patterns.

```
samples = [
  data/21AL.A_R1,
  data/21AL.A_R2,
  data/21AL.B_R1,
  data/21AL.B_R2
]
```

This quickly becomes overwhelming if you run many samples, and a more general approach is desirable. Snakemake provides this through the function `glob_wildcards`.

Here, Snakemake looks for sample names matching the pattern, and extracts the part of the file name not including 'fastq'.

```
samples = glob_wildcards("data/{sample}.fastq")
```

We can test this by adding a `print` statement on the second row `print(samples)`, and try running. Remember - we can use standard Python within the Snakemake workflow.

Now we are ready to run. To make this work, we need to update the workflow in a few places.

```
configfile: "config.yaml"
samples = [
  data/21AL.A_R1,
  data/21AL.A_R2,
  data/21AL.B_R1
]

rule all:
    input:
        expand("output/2_summaries/{sample}.tsv", sample=samples)

rule convert_fastq_to_fasta:
    input: "data/{sample}.fastq"
    output: "output/1_fasta/{sample}.fasta"
    shell:
        """
        python3 scripts/fastq_to_fasta.py --input {input} --output {output} --max_entries {config[settings][max_nbr_seqs]}
        """

rule get_sequence_measures:
    input:
        in_fasta="output/1_fasta/{sample}.fasta"
    output:
        out_fasta="output/2_summaries/{sample}.tsv"
    shell:
        """
        python3 scripts/retrieve_fasta_stats.py --input {input.in_fasta} --output {output.out_fasta}
        """
```

We use the `expand` command in the `rule all`, which generates a list of output files for all patterns. Note also that the pattern `{sample}` is now used throughout the code to specify the target sample names part of each file name.

![Workflow illustration, step 3](/post/2021-01-26-snakemake-a-friendly-introduction.en_files/step3_dag.svg)

## Exercise

1. Try the list-syntax, and see if you can run only a subset of the samples.
2. Try using the command `snakemake --dag | dot -Tsvg > dag.svg` to generate a graph which illustrates the different executed commands. Does the graph look like you expected?
3. (Bonus) If you are proficient in Python, feel free to explore the `retrieve_fasta_stats` script, and calculate further stats for the FASTA-files!

# Step 4 - Combining multiple files into one

At this point we will end up with a set of processed files, each containing useful information about each FASTA file. It is still a bit difficult to overview though, and a common task is to combine these different files into a single one, which then could be used for further inspection, statistics and visualizations.

```
configfile: "config.yaml"
samples = [
  data/21AL.A_R1,
  data/21AL.A_R2,
  data/21AL.B_R1
]

rule all:
    input:
        "output/3_combined/combined.tsv"

rule convert_fastq_to_fasta:
    input: "data/{sample}.fastq"
    output: "output/1_fasta/{sample}.fasta"
    shell:
        """
        python3 scripts/fastq_to_fasta.py --input {input} --output {output} --max_entries {config[settings][max_nbr_seqs]}
        """

rule get_sequence_measures:
    input:
        in_fasta="output/1_fasta/{sample}.fasta"
    output:
        out_fasta="output/2_summaries/{sample}.tsv"
    shell:
        """
        python3 scripts/retrieve_fasta_stats.py --input {input.in_fasta} --output {output.out_fasta}
        """

rule combine_sequence_measures:
    input: expand("output/2_summaries/{sample}.tsv", sample=samples)
    output: "output/3_combined/combined.tsv"
    shell:
        """
        python3 scripts/combine_files.py --input {input} --output {output}
        """
```

Now we have successfully generated a joint file with statistics calculated for each of the original files.

![Workflow illustration, step 4](/post/2021-01-26-snakemake-a-friendly-introduction.en_files/step4_dag.svg)

## Exercises

1. Try using the command `snakemake --dag | dot -Tsvg > dag.svg` to generate a graph which illustrates the different executed commands. Does the graph look like you expected?

# Step 5 - Visualizing and wrapping up

As a final step, we would like to generate visualizations from these measures. Here, we could commonly also calculate statistics, or combine with other data. 

```
configfile: "config.yaml"
samples = [
  data/21AL.A_R1,
  data/21AL.A_R2,
  data/21AL.B_R1
]

rule all:
    input:
        "output/4_visuals/visual_summary.png"

rule convert_fastq_to_fasta:
    input: "data/{sample}.fastq"
    output: "output/1_fasta/{sample}.fasta"
    shell:
        """
        python3 scripts/fastq_to_fasta.py --input {input} --output {output} --max_entries {config[settings][max_nbr_seqs]}
        """

rule get_sequence_measures:
    input:
        in_fasta="output/1_fasta/{sample}.fasta"
    output:
        out_fasta="output/2_summaries/{sample}.tsv"
    shell:
        """
        python3 scripts/retrieve_fasta_stats.py --input {input.in_fasta} --output {output.out_fasta}
        """

rule combine_sequence_measures:
    input: expand("output/2_summaries/{sample}.tsv", sample=samples)
    output: "output/3_combined/combined.tsv"
    shell:
        """
        python3 scripts/combine_files.py --input {input} --output {output}
        """

rule visualize_summary:
    input: "output/3_combined/combined.tsv"
    output: "output/4_visuals/visual_summary.png"
    shell:
        """
        python3 scripts/visualize_summary.py \
            --input {input} \
            --output {output} \
            --nbr_bins {config[plotting][number_bins]} \
            --nbr_cols {config[plotting][number_cols]} \
            --title {config[plotting][title]}
        """

```

![Workflow illustration, step 5](/post/2021-01-26-snakemake-a-friendly-introduction.en_files/step5_dag.svg)

## Exercises

1. Try using the command `snakemake --dag | dot -Tsvg > dag.svg` to generate a graph which illustrates the different executed commands. Does the graph look like you expected?
2. Config file - move the paths of the files to this file, and also specify how many threads should be used to avoid overwhelming the server.
3. (Bonus) If comfortable with Python, further adjust the 'visualize_summary.py' script to generate more output visualizations. This could also be done using an R script.

# More complex examples, and reading materials

These are some of the fundamentals. Of course, there are further variations on this, and in real life you might need particular solutions for particular situations. Worry not - most likely many other people have encountered the same issues before. If you google well, you will most likely find the answers. Also, here is a great page summarizing many of the common questions you might encounter:

https://snakemake.readthedocs.io/en/stable/project_info/faq.html

A more extensive tutorial is found here:

https://snakemake.readthedocs.io/en/stable/tutorial/tutorial.html#tutorial

Also, if you want to see a more extensive real-world example of Snakemake workflow, the NBIS organization (the National Bioinformatics Infrastructure in Sweden) has one here designed for metagenomics analyses:

https://github.com/NBISweden/nbis-meta