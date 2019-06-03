---


---

<h1 id="kraken2">Kraken2</h1>
<ul>
<li>Author: Dreycey Albin</li>
<li>Date:  05/13/2019</li>
<li>Updates:</li>
<li>Kraken2 is a software used for taxonomic classification of a sample given the reads</li>
<li>documentation (website): <a href="https://ccb.jhu.edu/software/kraken2/">https://ccb.jhu.edu/software/kraken2/</a></li>
<li>documentation (website-manual): <a href="https://ccb.jhu.edu/software/kraken2/index.shtml?t=manual">Kraken2 manual</a></li>
</ul>
<h2 id="setting-up-bind-path-singularity">Setting up bind path (singularity)</h2>
<ul>
<li>export SINGULARITY_BINDPATH=“kraken2_bind_directory”</li>
<li>export SINGULARITY_BINDPATH="/home/da39/Dreycey_scratch/kraken2_testing/"</li>
</ul>
<h2 id="building-the-kraken-database-singularity">Building the Kraken Database (singularity)</h2>
<pre><code>singularity exec kraken2.simg kraken2-build --standard --threads 30 --db kraken2database/
</code></pre>
<p><em>Note:</em> Can’t direct bindpath to the current directory for some reason.<br>
–&gt; only does the downloading in my home folder?</p>
<pre><code>kraken2-build --standard --threads 30 --db kraken2database/
</code></pre>
<ul>
<li>Setting up Kraken2 using github</li>
</ul>
<h2 id="installation-build-from-source">installation (Build from source)</h2>
<pre><code>conda install kraken2
</code></pre>
<p>OR</p>
<pre><code>git clone https://github.com/DerrickWood/kraken2
cd kraken2/
mkdir KRAKEN2STUFF
./install_kraken2.sh KRAKEN2STUFF/
</code></pre>
<p>–&gt; will get a “Kraken 2 installation complete.” message</p>
<ul>
<li>installing all of the environment variables for kraken:</li>
</ul>
<pre><code># for Kraken2

export PATH=$PATH:/home/da39/Dreycey/DHS_testing_programs/kraken2/KRAKEN2STUFF
</code></pre>
<h3 id="using-a-singularity-image">Using a singularity image</h3>
<p>Krista found that the following command works for downloading the singularity image:</p>
<pre><code>singularity exec -B ${PWD}/:/tmp kraken2-2.0.8_beta--pl526h6bb024c_0.simg kraken2 --db /tmp/minikraken2_v2_8GB_201904_UPDATE --paired /tmp/cDNA_trim30_1.fq. /tmp/cDNA_trim30_2.fq.gz --gzip-compressed --use-names --report 
/tmp/cDNA_Mix_Trim30_Kraken2_Report.txt
</code></pre>
<h2 id="options-for-building-the-database">options for building the database:</h2>
<h3 id="building-the-standard-db">Building the standard DB</h3>
<ul>
<li>Standard database build</li>
<li>2 step process: <strong>(1)</strong> estimation of hash table size needed; <strong>(2)</strong> populating the hash table with taxonomic mapping information</li>
</ul>
<pre><code>./scripts/kraken2-build --standard --threads 32 --db Kraken2DB
</code></pre>
<pre><code>cd Kraken2DB/;
rm database.kraken;
</code></pre>
<p><em>Note</em>: there will be three different databases left after building the database:<br>
(from website)</p>
<ul>
<li><code>hash.k2d</code>: Contains the minimizer to taxon mappings</li>
<li><code>opts.k2d</code>: Contains information about the options used to build the database</li>
<li><code>taxo.k2d</code>: Contains taxonomy information used to build the database<br>
–&gt; These are the only files needed after build, everything else may be deleted.</li>
</ul>
<p><em>Note_2</em>: This database will take around 100 GB to build. Most of this is in the temporary files used for building the database. These can be deleted after building the database.</p>
<h3 id="building-a-custom-kraken2-database">Building a <em>custom</em> kraken2 database</h3>
<p>1st download the NCBI taxonomy map:</p>
<pre><code>kraken2-build --download-library bacteria --db $DBNAME
</code></pre>
<p>2nd, build the library for all of the different taxon.</p>
<pre><code>kraken2-build --download-library archaea --db $DBNAME
kraken2-build --download-library viral --db $DBNAME
</code></pre>
<p>The list of potential libraries to install is as follows:</p>
<ul>
<li><code>archaea</code>: RefSeq complete archaeal genomes/proteins</li>
<li><code>bacteria</code>: RefSeq complete bacterial genomes/proteins</li>
<li><code>plasmid</code>: RefSeq plasmid nucleotide/protein sequences</li>
<li><code>viral</code>: RefSeq complete viral genomes/proteins</li>
<li><code>human</code>: GRCh38 human genome/proteins</li>
<li><code>fungi</code>: RefSeq complete fungal genomes/proteins</li>
<li><code>plant</code>: RefSeq complete plant genomes/proteins</li>
<li><code>protozoa</code>: RefSeq complete protozoan genomes/proteins</li>
<li><code>nr</code>: NCBI non-redundant protein database</li>
<li><code>nt</code>: NCBI non-redundant nucleotide database</li>
<li><code>env_nr</code>: NCBI non-redundant protein database with sequences from large environmental sequencing projects</li>
<li><code>env_nt</code>: NCBI non-redundant nucleotide database with sequences from large environmental sequencing projects</li>
<li><code>UniVec</code>: NCBI-supplied database of vector, adapter, linker, and primer sequences that may be contaminating sequencing projects and/or assemblies</li>
<li><code>UniVec_Core</code>: A subset of UniVec chosen to minimize false positive hits to the vector database</li>
</ul>
<h2 id="example-run-for-kraken2-using-standard-db">Example run for kraken2 (using standard DB)</h2>
<h3 id="downloading-reads-to-a-metagenomic-sample">Downloading reads to a metagenomic sample</h3>
<ul>
<li>example are soil reads from: <a href="https://www.ncbi.nlm.nih.gov/sra?linkname=bioproject_sra_all&amp;from_uid=444435">Soil reads data</a><br>
Run the following command:</li>
</ul>
<pre><code>fastq-dump SRX3886494;
</code></pre>
<p><em>Note</em>: Must have SRA tools installed to run fastq-dump:<br>
<a href="https://github.com/ncbi/sra-tools">https://github.com/ncbi/sra-tools</a></p>
<h3 id="running-kraken2-command-used">Running Kraken2 (command used)</h3>
<pre><code>kraken2 --db Kraken2DB --threads 30 --classified-out classified_sequences.txt --unclassified-out unclassified_sequences.txt --fastq-input SRX3886494.fasta
</code></pre>
<p>Krista used:</p>
<pre><code>kraken2 --db Kraken2DB --threads 30 --classified-out classified_sequences.txt --unclassified-out unclassified_sequences.txt --fastq-input SRX3886494.fasta
</code></pre>
<h3 id="output-files">Output files</h3>
<h2 id="databases-used-for-kraken2">Databases used for Kraken2</h2>
<p>From Krista:</p>
<pre><code>I selected the minikraken2_v2_8GB_201904_UPDATE database because it was smaller and might be good for distribution ([https://ccb.jhu.edu/software/kraken2/downloads.shtml](https://ccb.jhu.edu/software/kraken2/downloads.shtml)), but hopefully we can build this in a way that end users can use the larger standard or other custom databases with Kraken2 as needed.
</code></pre>
<h2 id="errors">Error(s)</h2>
<p>I keeep experiencing the following error when building the database"</p>
<pre><code>mv: replace 'assembly_summary.txt', overriding mode 0444 (r--r--r--)? y
Step 1/2: Performing rsync file transfer of requested files
Rsync file transfer complete.
Step 2/2: Assigning taxonomic IDs to sequences
Processed 1 project (594 sequences, 3.26 Gbp)... done.
All files processed, cleaning up extra sequence files... done, library complete.
Downloading UniVec_Core data from server... done.
Adding taxonomy ID of 28384 to all sequences... done.
Masking low-complexity regions of downloaded library... done.
Creating sequence ID to taxonomy ID map (step 1)...
Sequence ID to taxonomy ID map complete. [0.020s]
Estimating required capacity (step 2)...
/scratch/Dreycey/DHS_testing_programs/kraken2/scripts/build_kraken2_db.sh: line 101: estimate_capacity: command not found
xargs: cat: terminated by signal 13
</code></pre>
<p>running the following command:</p>
<pre><code>ls * | grep est
</code></pre>
<p>returned the following files:</p>
<pre><code>estimate_capacity
estimate_capacity
estimate_capacity.cc
mmtest.cc
</code></pre>
<p>Problem fixed! It required uninstalling conda to debug:</p>
<pre><code>conda uninstall kraken2
</code></pre>
<p>then adding the following to the ~/.bashrc:</p>
<pre><code># for Kraken2

export PATH=$PATH:/home/da39/Dreycey/DHS_testing_programs/kraken2/KRAKEN2STUFF
</code></pre>
<h3 id="problem-continued..">problem continued…</h3>
<pre><code>/scratch/Dreycey/DHS_testing_programs/kraken2/scripts/build_kraken2_db.sh: line 101: estimate_capacity: command not found

xargs: cat: terminated by signal 13

(base) **da39@gho**:**~/Dreycey/DHS_testing_programs/kraken2**$ estimate_capacity

estimate_capacity: command not found
</code></pre>
<p>It’s a problem now of the screen not having the env variable<br>
hopefully running the following will fix the problem</p>
<pre><code>source ~/.bashrc
</code></pre>
<h2 id="general-commands">General commands</h2>
<p>The general command for running Kraken2 is the following:</p>
<pre><code>kraken2 --db $DBNAME seqs.fa
</code></pre>
<p>This command may be used with the commands/options listed below.</p>
<p>v to a file for later processing, using the  <code>--classified-out</code>  and  <code>--unclassified-out</code>  switches, respectively.</p>
<ul>
<li>
<p><strong>Output redirection</strong>: Output can be directed using standard shell redirection (<code>|</code>  or  <code>&gt;</code>), or using the  <code>--output</code>  switch.</p>
</li>
<li>
<p><strong>FASTQ input</strong>: Input is normally expected to be in FASTA format, but you can classify FASTQ data using the  <code>--fastq-input</code>  switch.</p>
</li>
<li>
<p><strong>Compressed input</strong>: Kraken 2 can handle gzip and bzip2 compressed files as input by specifying the proper switch of  <code>--gzip-compressed</code>  or  <code>--bzip2-compressed</code>.</p>
</li>
<li>
<p><strong>Input format auto-detection</strong>: If regular files are specified on the command line as input, Kraken 2 will attempt to determine the format of your input prior to classification. You can disable this by explicitly specifying  <code>--fasta-input</code>,  <code>--fastq-input</code>,  <code>--gzip-compressed</code>, and/or  <code>--bzip2-compressed</code>  as appropriate. Note that use of the character device file  <code>/dev/fd/0</code>  to read from standard input (aka  <code>stdin</code>) will  <strong>not</strong>  allow auto-detection.</p>
</li>
<li>
<p><strong>Paired reads</strong>: Kraken 2 provides an enhancement over Kraken 1 in its handling of paired read data. Rather than needing to concatenate the pairs together with an  <code>N</code>  character between the reads, Kraken 2 is able to process the mates individually while still recognizing the pairing information. Using the  <code>--paired</code>  option to  <code>kraken2</code>  will indicate to  <code>kraken2</code>  that the input files provided are paired read data, and data will be read from the pairs of files concurrently.</p>
<p>Usage of  <code>--paired</code>  also affects the  <code>--classified-out</code>  and  <code>--unclassified-out</code>  options; users should provide a  <code>#</code>  character in the filenames provided to those options, which will be replaced by  <code>kraken2</code>  with “<code>_1</code>” and “<code>_2</code>” with mates spread across the two files appropriately. For example:</p>
<pre><code>kraken2 --paired --classified-out cseqs#.fq seqs_1.fq seqs_2.fq
</code></pre>
<p>will put the first reads from classified pairs in  <code>cseqs_1.fq</code>, and the second reads from those pairs in  <code>cseqs_2.fq</code>.<br>
To get a full list of options, use  <code>kraken2 --help</code>.</p>
</li>
</ul>

