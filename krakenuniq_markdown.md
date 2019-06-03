---


---

<h1 id="krakenuniq">KrakenUniq</h1>
<ul>
<li>Author: Dreycey Albin</li>
<li>Date: 05/13/2019</li>
<li>Updates:</li>
<li>KrakenUniq is a software for taxonomic classification. It decreases false positives by making sure reads are dispersed over the entire genome of the bacteria being classified. It does so by estimating the cardinality of a unique kmer set using a hyperloglog algorithm (HLL).</li>
<li>documentation (website): <a href="https://github.com/fbreitwieser/krakenuniq">https://github.com/fbreitwieser/krakenuniq</a></li>
<li>documentation (publication): <a href="https://genomebiology.biomedcentral.com/articles/10.1186/s13059-018-1568-0">https://genomebiology.biomedcentral.com/articles/10.1186/s13059-018-1568-0</a></li>
<li>documentation (website-manual): <a href="https://github.com/fbreitwieser/krakenuniq/blob/master/MANUAL.md">KrakenUniq MANUAL</a></li>
</ul>
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
<p>You are able to run KrakenUniq on existing Kraken databases. Therefore, it may be more memory efficient to use the databases already built for use with Kraken (Doesn’t work with kraken2). In addition, there is the possibility of building a custom database, so as long as the user has the following files: (1) sequence files in FASTA format; (2) Mapping files; (3) NCBI taxonomy files (or custom).</p>
<h3 id="building-database-using-standard-download">Building database using standard download</h3>
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
<h2 id="example">Example</h2>
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
<p>Ah, it’s because Kraken2 DB wasn’t finished downloading</p>
</li>
</ul>
<hr>
