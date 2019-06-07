---


---

<h1 id="krakenuniq">KrakenUniq</h1>
<ul>
<li>Author: Dreycey Albin</li>
<li>Date: 05/13/2019</li>
<li>Updates:</li>
<li>KrakenUniq is a software for taxonomic classification. It decreases false positives by making sure reads are dispersed over the entire genome of the bacteria being classified. It does so by estimating the cardinality of a unique kmer set using a hyperloglog algorithm (HLL). Should be noted that the databases can be built using krakenuniq or using a standard kraken 1.0 database. There is also the flexibility to build a custom database.</li>
<li>documentation (website): <a href="https://github.com/fbreitwieser/krakenuniq">https://github.com/fbreitwieser/krakenuniq</a></li>
<li>documentation (publication): <a href="https://genomebiology.biomedcentral.com/articles/10.1186/s13059-018-1568-0">https://genomebiology.biomedcentral.com/articles/10.1186/s13059-018-1568-0</a></li>
<li>documentation (website-manual): <a href="https://github.com/fbreitwieser/krakenuniq/blob/master/MANUAL.md">KrakenUniq MANUAL</a></li>
</ul>
<p>CHECK OUT THE FOLLOWING:<br>
<a href="ftp://ftp.ccb.jhu.edu/pub/software/krakenuniq/Databases/">ftp://ftp.ccb.jhu.edu/pub/software/krakenuniq/Databases/</a></p>
<h2 id="installation-of-krakenuniq">installation of KrakenUniq</h2>
<pre><code>git clone https://github.com/fbreitwieser/krakenuniq
cd krakenuniq/
mkdir KrakenUniqInstallDir
./install_krakenuniq.sh KrakenUniqInstallDir/
</code></pre>
<p>OR</p>
<pre><code>conda install krakenuniq
</code></pre>
<p>OR</p>
<p><a href="https://quay.io/organization/biocontainers">https://quay.io/organization/biocontainers</a></p>
<h2 id="building-database-for-krakenuniq">Building database for KrakenUniq</h2>
<p>You are able to run KrakenUniq on existing Kraken databases. Therefore, it may be more memory efficient to use the databases already built for use with Kraken (<strong>Doesn’t work with kraken2</strong>). In addition, there is the possibility of building a custom database, so as long as the user has the following files: (1) sequence files in FASTA format; (2) Mapping files; (3) NCBI taxonomy files (or custom).</p>
<h3 id="building-database-using-standard-download-was-not-able-to-build-database-using-krakenuniq">Building database using standard download [Was not able to build database using KrakenUniq?]</h3>
<ul>
<li>run install script</li>
</ul>
<pre><code>mkdir install_location
./install_krakenuniq.sh -j install_location/
</code></pre>
<ul>
<li>Add the following to your bashrc:</li>
</ul>
<pre><code>#For krakenUniq
export PATH=$PATH:/home/da39/Dreycey/DHS_testing_programs/KRAKENUNIQ/krakenuniq/install_location
</code></pre>
<ul>
<li>Added the following to a bash file, then ran the file:</li>
</ul>
<pre><code>	scripts/krakenuniq-download --db KrakenUniqDB_DIR taxonomy;
	scripts/krakenuniq-download --db KrakenUniqDB_DIR --threads 10 --dust refseq/bacteria refseq/archaea;
	scripts/krakenuniq-download --db KrakenUniqDB_DIR refseq/vertebrate_mammalian/Chromosome/species_taxid=9606;
	scripts/krakenuniq-download --db KrakenUniqDB_DIR refseq/viral/Any viral-neighbors;
	scripts/krakenuniq-download --db KrakenUniqDB_DIR --dust microbial-nt;
	scripts/krakenuniq-build --db KrakenUniqDB_DIR --kmer-len 31 --threads 10 --taxids-for-genomes --taxids-for-sequences;
</code></pre>
<ul>
<li>make sure to give privileges;<br>
I ran “chmod u+x {bash file name}” to get it ready for running:</li>
</ul>
<pre><code>chmod u+x makeDB_script.sh
</code></pre>
<h2 id="example-using-kraken-1.0-database">Example (using Kraken 1.0 database)</h2>
<h3 id="downloading-reads-to-a-metagenomic-sample">Downloading reads to a metagenomic sample</h3>
<ul>
<li>example are soil reads from: <a href="https://www.ncbi.nlm.nih.gov/sra?linkname=bioproject_sra_all&amp;from_uid=444435">Soil reads data</a><br>
Run the following command:</li>
</ul>
<pre><code>fastq-dump SRX3886494;
</code></pre>
<p><em>Note</em>: Must have SRA tools installed to run fastq-dump:<br>
<a href="https://github.com/ncbi/sra-tools">https://github.com/ncbi/sra-tools</a></p>
<h3 id="running-krakenuniq-from-the-command-line">Running KrakenUniq from the command line</h3>
<pre><code>./install_location/krakenuniq --db //rdf/Dreycey/krakenDB/ --fastq-input SRX3886494.fastq  --threads 25 --report-file output_2.report
</code></pre>
<h3 id="output-files">Output files:</h3>
<pre><code>output_2.report
</code></pre>
<h2 id="problems-fixed">Problems [Fixed]</h2>
<ul>
<li>
<p>Got an error message: krakenuniq: database (“KrakenUniqDB_DIR/”) does not contain necessary file database.kdb</p>
</li>
<li>
<p>Ah, it’s because Kraken2 is not compatible</p>
</li>
</ul>
<hr>
<h2 id="parameters">Parameters</h2>
<pre><code>Usage: krakenuniq --report-file FILENAME [options] &lt;filename(s)&gt;
Options:
--db NAME Name for Kraken DB (default: none)
--threads NUM Number of threads (default: 1)
--fasta-input Input is FASTA format
--fastq-input Input is FASTQ format
--gzip-compressed Input is gzip compressed
--bzip2-compressed  Input is bzip2 compressed
--hll-precision INT Precision for HyperLogLog k-mer cardinality estimation, between 10 and 18 (default: 12)
--exact Compute exact cardinality instead of estimate (slower, requires memory proportional to cardinality!)
--quick Quick operation (use first hit or hits)
--min-hits NUM  In quick op., number of hits req'd for classification
NOTE: this is ignored if --quick is not specified
--unclassified-out FILENAME
Print unclassified sequences to filename
--classified-out FILENAME
Print classified sequences to filename
--output FILENAME Print output to filename (default: stdout); "off" will
suppress normal output
--only-classified-output
Print no Kraken output for unclassified sequences
--preload Loads DB into memory before classification
--paired  The two filenames provided are paired-end reads
--check-names Ensure each pair of reads have names that agree
with each other; ignored if --paired is not specified
--help  Print this message
--version Print version information

Experimental:
--uid-mapping Map using UID database
</code></pre>
<h2 id="kraken_uniq-for-snakemakesingularity">Kraken_Uniq for Snakemake&amp;Singularity</h2>
<h3 id="install-docker-container">Install docker container</h3>
<pre><code>singularity pull docker://quay.io/biocontainers/krakenuniq:0.5.8--pl526he860b03_0;
</code></pre>
<h3 id="installing-databases-for-krakenuniq">Installing databases for KrakenUniq</h3>
<h4 id="install-minikraken1db-note-the-regular-db-isnt-too-large">Install minikraken1DB (note: the regular DB isn’t too large)</h4>
<pre><code>wget https://ccb.jhu.edu/software/kraken/dl/minikraken_20171019_8GB.tgz;
tar -zxvf minikraken_20171019_8GB.tgz;
mv  minikraken_20171019_8GB kraken1DB;
rm minikraken_20171019_8GB.tgz;
</code></pre>
<h4 id="installing-the-krakenuniqdb-doesnt-work-use-minikraken1db">Installing the krakenUniqDB (doesn’t work, use minikraken1DB)</h4>
<pre><code>	scripts/krakenuniq-download --db KrakenUniqDB_DIR taxonomy;
	scripts/krakenuniq-download --db KrakenUniqDB_DIR --threads 10 --dust refseq/bacteria refseq/archaea;
	scripts/krakenuniq-download --db KrakenUniqDB_DIR refseq/vertebrate_mammalian/Chromosome/species_taxid=9606;
	scripts/krakenuniq-download --db KrakenUniqDB_DIR refseq/viral/Any viral-neighbors;
	scripts/krakenuniq-download --db KrakenUniqDB_DIR --dust microbial-nt;
	scripts/krakenuniq-build --db KrakenUniqDB_DIR --kmer-len 31 --threads 10 --taxids-for-genomes --taxids-for-sequences;
</code></pre>
<p><strong>NOTE</strong>: More features available with the full kraken1DB  (500GB)</p>
<h3 id="install-example-data-to-use">Install example data to use</h3>
<pre><code>//rdf/software/sratoolkit.2.9.6-ubuntu64/bin/fastq-dump SRX200675 --split-files;
</code></pre>
<h3 id="running-kraken_uniq">Running Kraken_Uniq</h3>
<pre><code>mkdir krakenUniqoutput;
</code></pre>
<h4 id="we-will-use-the-general-command-below">We will use the general command below:</h4>
<pre><code>singularity exec -B ${PWD}/:/tmp kraken2-2.0.8_beta--pl526h6bb024c_0.simg kraken2 --db &lt;path to DB&gt; --threads &lt;NUM&gt; --paired &lt;pairedend 1&gt;  &lt;pairedned 2&gt; --output &lt;output file&gt; --report &lt;report file&gt;;
</code></pre>
<h4 id="here-is-the-command-for-running-the-shakya-dataset">Here is the command for running the Shakya dataset:</h4>
<pre><code>export SINGULARITY_BINDPATH=${PWD}/:/tmp;
</code></pre>
<pre><code>singularity exec -B ${PWD}/:/tmp krakenuniq-0.5.8--pl526he860b03_0.simg krakenuniq --paired --db /tmp/kraken1DB/ --threads 15 --hll-precision 12 --report-file /tmp/krakenUniqoutput/krakenuniq_report.report --unclassified-out /tmp/krakenUniqoutput/krakenuniq_unclass.txt --classified-out /tmp/krakenUniqoutput/krakenuniq_class.txt -output /tmp/krakenUniqoutput/krakenuniq.out /tmp/SRX200675_1.fastq /tmp/SRX200675_2.fastq;
</code></pre>
<h4 id="users-should-have-access-to-the-following">Users should have access to the following:</h4>
<pre><code>--threads &lt;INT&gt;
--db &lt;path to Kraken1DB&gt;
--paired (which is a switch)
 --hll-precision (limit between 10 and 18) 
</code></pre>

