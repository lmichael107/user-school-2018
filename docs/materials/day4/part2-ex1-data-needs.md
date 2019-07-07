---
status: tested
---

Understanding Data Requirements
===============================


Background
----------

This exercise's goal is to learn to think critically about an application's data needs, especially before submitting a large batch of jobs or using tools for delivering large data to jobs. In this exercise we will attempt to understand the input and output of the bioinformatics application [BLAST](http://blast.ncbi.nlm.nih.gov/), which you used yesterday in [Exercise 1.2](/materials/day3/part1-ex2-precompiled.md).

### Setup

-   Make sure you are logged into `training.osgconnect.net`.
-   Navigate to your `stash` directory within the home directory, and create a directory for this exercise named `thur-blast`.

#### Copy the blast executables:

``` console
user@training $ wget http://proxy.chtc.wisc.edu/SQUID/osgschool18/ncbi-blast-2.7.1+-x64-linux.tar.gz
user@training $ tar xzf ncbi-blast-2.7.1+-x64-linux.tar.gz
```

#### Copy the Input Files

To run BLAST, we need an input file and reference database. For this example, we'll use the "pdbaa" database, which contains sequences for the protein structure from the Protein Data Bank. For our input file, we'll use an abbreviated fasta file with mouse genome information.

1.  Download these files to your current directory:

``` console
user@training $ wget http://proxy.chtc.wisc.edu/SQUID/osgschool18/pdbaa.tar.gz
user@training $ wget http://proxy.chtc.wisc.edu/SQUID/osgschool18/mouse.fa
```

1.  Untar the `pdbaa` database:

``` console
user@training $ tar -xzf pdbaa.tar.gz
```

Understanding blast
-------------------

Remember that `blastx` is executed in a command like the following:

``` console
user@training $ blastx -db %RED%database_rootname%ENDCOLOR% -query %RED%input_file%ENDCOLOR% -out %RED%results_file%ENDCOLOR%
```

In the above, the `input_file` is a file containing a number of genetic sequences, and the database that these are compared against is made up of several files that begin with the same root name (we previously used the "pdbaa" database, whose files all begin with `pdbaa`). The output from this analysis will be printed to a results file that is also indicated in the command.

Adding up data needs
--------------------

Looking at the files from the `blastx` jobs you ran yesterday, add up the amount of data that was needed for running the job. (If you deleted any of the files from yesterday, just resubmit the job before proceeding.) Here are the commands that will be most useful to you:

How to see the size of a specific file:

``` console
user@training $ ls -lh %RED%<FILE>%ENDCOLOR%
```

How to see the size of all files in the current directory:

``` console
user@training $ ls -lh
```

How to determine the total amount of data in the current directory: 

``` console
user@training $ du -sh %RED%<DIRECTORY>%ENDCOLOR%
```

### Input requirements

Looking at yesterday's exercise, total up the amount of data in all of the files necessary to run the `blastx` job (which will include the executable, itself). Write down this number. Also take note of how much total data in in the `pdbaa` directory.  Remember, `blastx` reads the un-compressed `pdbaa` files.

!!! note "Compressed Files"
    Remember, `blastx` reads the un-compressed `pdbaa` files.

### Output requirements

The output that we care about from `blastx` is saved in the file whose name is indicated after the `-out` argument to `blastx`. However, remember that HTCondor also creates the error, output, and log files, which you'll need to add up, too. Are there any other files? Total all of these together, as well.

Talk about this as a group!
---------------------------

Once you have completed the above tasks, we'll talk about the totals as a group.

-   How much disk space is required on the submit server for one blastx run with the input file you used before? (Input data)
-   How *many* files are needed and created for each run? (Output data)
-   Assuming that each file is read completely by BLAST, and since you know how long blastx runs (time it):
    -   At what rate are files read in?
    -   How many MB/s?
-   How much total disk space would be necessary on the submit server to run 10 jobs? (remember that some of the files will be shared by all 10 jobs, and will not be multiplied)

Up next!
--------

Next you will create a HTCondor submit script to transfer the Blast input files in order to run Blast on a worker nodes. [Next Exercise](/materials/day4/part2-ex2-file-transfer.md)




