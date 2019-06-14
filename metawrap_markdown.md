---


---

<h1 id="metawrap">MetaWRAP</h1>
<ul>
<li>Author: Dreycey Albin</li>
<li>Date:  05/13/2019</li>
<li>Updates:</li>
<li>The purpose of metawrap is to function as a pipeline for analysis of metagenomics data</li>
<li>documentation (website): <a href="https://github.com/bxlab/metaWRAP">https://github.com/bxlab/metaWRAP</a></li>
<li>documentation (publication): <a href="https://microbiomejournal.biomedcentral.com/articles/10.1186/s40168-018-0541-1">metaWRAP publication</a></li>
</ul>
<h2 id="important-considerations">Important considerations</h2>
<ul>
<li><strong>NOTE</strong>: only supports kraken 1.0, not kraken 2.0. This is because kraken 2.0 does not generate the required database.kdb file</li>
</ul>
<h2 id="installation-of-metawrap">installation of MetaWRAP</h2>
<h4 id="first-create-a-conda-environement">First create a conda environement</h4>
<pre><code>conda create -y -n metawrap-env python=2.7
source activate metawrap-env
</code></pre>
<h4 id="install-dependencies-in-env">Install dependencies in env</h4>
<pre><code>conda config --add channels defaults
conda config --add channels conda-forge
conda config --add channels bioconda
conda config --add channels ursky
</code></pre>
<ul>
<li>install more of the dependencies required</li>
</ul>
<pre><code>conda install --only-deps -c ursky metawrap-mg
</code></pre>
<ul>
<li>install MetaWRAP</li>
</ul>
<pre><code>conda install -y -c ursky metawrap-mg
</code></pre>
<h3 id="activate-the-conda-environment">activate the conda environment</h3>
<pre><code>conda activate metawrap-env
</code></pre>
<h2 id="install-the-databases-needed-for-metawrap">Install the databases needed for MetaWRAP</h2>
<p>URL: <a href="https://github.com/bxlab/metaWRAP/blob/master/installation/database_installation.md">MetaWRAP database information</a></p>
<h4 id="finding-the-config-file">Finding the config file</h4>
<p>To direct metaWRAP to the correct databases for operation, one must modify the config file for metaWRAP. This is done by modifying the associated config file. To find where this config file is located on the system, use the following command to obtain the path :</p>
<pre><code>which config-metawrap
</code></pre>
<p>After following this path to the correct file, edit the file with the needed database paths. This will allow for the modules to find the right files. The paths to edit consist of: (1) ‘BLASTDB=’, (2) ‘TAXDUMP=’, (3) ‘BMTAGGER_DB=’, and (4) ‘KRAKEN_DB=’.</p>
<h2 id="config-file">Config file</h2>
<p>After everything is correctly installed, the paths are added to the config file. My config file looks like this:</p>
<pre><code># Paths to metaWRAP scripts (dont have to modify)
mw_path=$(which metawrap)
bin_path=${mw_path%/*}
SOFT=${bin_path}/metawrap-scripts
PIPES=${bin_path}/metawrap-modules

# CONFIGURABLE PATHS FOR DATABASES (see 'Databases' section of metaWRAP README for details)

# path to kraken standard database
KRAKEN_DB=//scratch/Maria/minikraken_20171019_8GB

# path to indexed human (or other host) genome (see metaWRAP website for guide). This includes .bitmask and .srprism files
BMTAGGER_DB=/home/da39/Dreycey_scratch/DHS_testing_programs/metaWRAP/BMTAGGER_INDEX

# paths to BLAST databases
BLASTDB=//rdf/tt40/dbs/ncbi/nt
TAXDUMP=///home/da39/Dreycey_scratch/DHS_testing_programs/metaWRAP/NCBI_tax
</code></pre>
<h2 id="trying-different-modules-within-metawrap">Trying different modules within metaWRAP</h2>
<h3 id="assembly">Assembly</h3>
<pre><code>metawrap assembly -1 SRR606249_subset10_1.fq.gz -2 SRR606249_subset10_2.fq.gz -o outdir -t 35 -l 1000 --megahit
</code></pre>
<ul>
<li>Looked at the output report for this assembly, and it looks like it worked as expected.</li>
</ul>
<h3 id="binning">binning</h3>
<pre><code> 	metawrap binning --concoct -a outdir/final_assembly.fasta -o binning_ouput -t 40 -m 70 -l 1000 SRR606249_subset10_1.fastq SRR606249_subset10_2.fastq 
</code></pre>
<ul>
<li>had to gzip -d {file.fq.gz} the files, then change the name from file.fq to file.fastq</li>
<li>Spits out: BINNING PIPELINE SUCCESSFULLY FINISHED!!!</li>
</ul>
<h4 id="parameters-for-binning">Parameters for binning</h4>
<pre><code>Options:
-a STR  metagenomic assembly file
-o STR  output directory
-t INT  number of threads (default=1)
-m INT  amount of RAM available (default=4)
-l INT  minimum contig length to bin (default=1000bp). Note: metaBAT will default to 1500bp minimum
--metabat2  bin contigs with metaBAT2
--metabat1  bin contigs with the original metaBAT
--maxbin2  bin contigs with MaxBin2
--concoct  bin contigs with CONCOCT
--universal  use universal marker genes instead of bacterial markers in MaxBin2 (improves Archaea binning)
--run-checkm  immediately run CheckM on the bin results (requires 40GB+ of memory)
--single-end  non-paired reads mode (provide *.fastq files)
--interleaved  the input read files contain interleaved paired-end reads
</code></pre>
<h3 id="kraken">Kraken</h3>
<pre><code>metawrap kraken -s all -t 30 -o krakenoutput_2/ SRR606249_subset10_1.fastq SRR606249_subset10_2.fastq
</code></pre>
<p>Output:</p>
<pre><code>Writing krakenoutput_2//kronagram.html...
########################################################################################################################
#####  FINISHED RUNNING KRAKEN PIPELINE!!! #####
########################################################################################################################
real  1m46.645s
user  8m12.338s
sys 0m39.084s
</code></pre>
<h3 id="blobology">Blobology</h3>
<p>The databases were reinstalled using the following scripts:</p>
<p>For NCBI taxonomy:</p>
<pre><code>mkdir NCBI_tax;
cd NCBI_tax;
wget ftp://ftp.ncbi.nlm.nih.gov/pub/taxonomy/taxdump.tar.gz;
tar -xvf taxdump.tar.gz;
</code></pre>
<p>For NCBI nt:</p>
<pre><code>mkdir NCBI_nt;
cd  NCBI_nt;
wget "ftp://ftp.ncbi.nlm.nih.gov/blast/db/nt.*.tar.gz";
for a in nt.*.tar.gz; do tar xzf $a; done
</code></pre>
<p>and the following command was used for blobology:</p>
<pre><code>metawrap blobology -a NCS_067_INTRO3_paired_scaffolds.fasta -o blobout -t 30 SRR606249_subset10_1.fastq SRR606249_subset10_2.fastq
</code></pre>
<p>If running correctly, the following will appear on the output screen:</p>
<pre><code>------------------------------------------------------------------------------------------------------------------------
----- blobplot text file saved to blobout/NCS_067_INTRO3_paired_scaffolds.blobplot -----
------------------------------------------------------------------------------------------------------------------------
########################################################################################################################

#####  MAKE FINAL BLOBPLOT IMAGES  #####

########################################################################################################################

------------------------------------------------------------------------------------------------------------------------

-----  making blobplots with phylogeny annotations (order, phylum, and superkingdom).  -----

------------------------------------------------------------------------------------------------------------------------

  

There were 15 warnings (use warnings() to see them)

null device

1

There were 12 warnings (use warnings() to see them)

null device

1

Warning messages:

1: Transformation introduced infinite values in continuous y-axis

2: Transformation introduced infinite values in continuous y-axis

3: Transformation introduced infinite values in continuous y-axis

4: Removed 8 rows containing missing values (geom_point).

5: Removed 20 rows containing missing values (geom_point).

6: Removed 2 rows containing missing values (geom_point).

null device

1

  

------------------------------------------------------------------------------------------------------------------------

-----  cleaning up...  -----

------------------------------------------------------------------------------------------------------------------------

  

  

########################################################################################################################

#####  BLOBPLOT PIPELINE FINISHED SUCCESSFULLY!!!  #####

########################################################################################################################

  

  

real  364m58.614s

user  73m28.347s

sys  66m16.586s
</code></pre>
<h3 id="output-for-blobout">Output for blobout:</h3>
<pre><code>blobplot_figures NCS_067_INTRO3_paired_scaffolds.fasta.3.bt2

NCS_067_INTRO3_paired_scaffolds.blobplot NCS_067_INTRO3_paired_scaffolds.fasta.4.bt2

**NCS_067_INTRO3_paired_scaffolds.fasta** NCS_067_INTRO3_paired_scaffolds.fasta.rev.1.bt2

NCS_067_INTRO3_paired_scaffolds.fasta.1.bt2  NCS_067_INTRO3_paired_scaffolds.fasta.rev.2.bt2

NCS_067_INTRO3_paired_scaffolds.fasta.2.bt2  NCS_067_INTRO3_paired_scaffolds.nt.1e-5.megablast
</code></pre>
<pre><code>singularity pull docker://quay.io/biocontainers/metawrap:1.1--0
</code></pre>
<h3 id="install-example-data-to-use">Install example data to use</h3>
<p>//rdf/software/sratoolkit.2.9.6-ubuntu64/bin/fastq-dump SRX200675 --split-files;</p>
<h3 id="quant_bins">QUANT_BINS</h3>
<pre><code>metawrap quant_bins -b binning_ouput/concoct_bins/ -a binning_ouput/work_files/assembly.fa -t 27 -o quant_bins SRR606249_subset10_1.fastq SRR606249_subset10_2.fastq
</code></pre>
<h3 id="bin-classification">Bin Classification</h3>
<ul>
<li>The example below is using the binning generating using “concoct”</li>
</ul>
<pre><code>metawrap classify_bins -b binning_ouput/concoct_bins/ -o bin_classify_2/ -t 50
</code></pre>
<h3 id="bin-annotation">Bin Annotation</h3>
<pre><code>metawrap annotate_bins -b binning_ouput/concoct_bins/ -t 15 -o annotaed_bins_out
</code></pre>
<h3 id="bin-refinement">Bin Refinement</h3>
<ul>
<li>Did not run the following, but assuming this should work:</li>
</ul>
<pre><code>metawrap bin_refinement -t 15 -o bin_refinement_out -m 40 -c 70 -x 1- -A binning_ouput/concoct_bins/
</code></pre>
<h4 id="parameters">Parameters</h4>
<pre><code>Options:
-o STR  output directory
-t INT  number of threads (default=1)
-m INT  memory available (default=40)
-c INT  minimum % completion of bins [should be &gt;50%] (default=70)
-x INT  maximum % contamination of bins that is acceptable (default=10)
-A STR  folder with metagenomic bins (files must have .fa or .fasta extension)
-B STR  another folder with metagenomic bins
-C STR  another folder with metagenomic bins
--skip-refinement  dont use binning_refiner to come up with refined bins based on combinations of binner outputs
--skip-checkm  dont run CheckM to assess bins
--skip-consolidation  choose the best version of each bin from all bin refinement iteration
--keep-ambiguous  for contigs that end up in more than one bin, keep them in all bins (default: keeps them only in the best bin)
--remove-ambiguous  for contigs that end up in more than one bin, remove them in all bins (default: keeps them only in the best bin)
--quick  adds --reduced_tree option to checkm, reducing runtime, especially with low memory
</code></pre>
<h3 id="reassembling-the-bins">Reassembling the bins</h3>
<pre><code>metawrap reassemble_bins -b binning_ouput/concoct_bins/ -o reassemble_bins_out -1 SRR606249_subset10_1.fastq -2 SRR606249_subset10_2.fastq -t 20 -m 40 -c 70 -x 10 -l 500
</code></pre>

