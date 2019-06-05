---


---

<h1 id="kraken-1.0">Kraken 1.0</h1>
<ul>
<li>Author: Dreycey Albin</li>
<li>Date:  05/13/2019</li>
<li>Updates:</li>
<li>Kraken2 is a software used for taxonomic classification of a sample given the reads</li>
<li>documentation (website):</li>
<li>documentation (website-manual): <a href="http://ccb.jhu.edu/software/kraken/MANUAL.html">KRAKENMANUAL.html</a></li>
</ul>
<h2 id="setting-up-bind-path-singularity">Setting up bind path (singularity)</h2>
<h2 id="installing">Installing</h2>
<h3 id="install-source-using-the-following-commands">install source using the following commands:</h3>
<pre><code>git clone https://github.com/DerrickWood/kraken;
cd kraken;
./install_kraken.sh Kraken_installation_directory;
</code></pre>
<h3 id="install-kraken-singularity-container-using-the-following-commands">Install Kraken singularity container using the following commands:</h3>
<pre><code>singularity pull docker://quay.io/biocontainers/kraken:0.10.6_eaf8fb68--pl5.22.0_4;
</code></pre>
<p>Ran the tool using the singularoty image but it produced the following error:</p>
<pre><code>singularity exec -B {PWD}:/tmp/ kraken-0.10.6_eaf8fb68--pl5.22.0_4.simg kraken --db //rdf/Dreycey/krakenDB --threads 10 --fastq-input paired_1.fastq paired_2.fastq
</code></pre>
<pre><code>WARNING: Not mounting requested bind point (already mounted in container): /tmp/
perl: warning: Setting locale failed.
perl: warning: Please check that your locale settings:
LANGUAGE = (unset),
LC_ALL = (unset),
LANG = "en_US.UTF-8"
are supported and installed on your system.
perl: warning: Falling back to the standard locale ("C").
kraken: database ("../../../../../rdf/Dreycey/krakenDB") does not contain necessary file database.kdb
</code></pre>
<p>However the following command worked fine:</p>
<pre><code>./kraken --db //rdf/Dreycey/krakenDB --threads 10 --fastq-input //rdf/Dreycey/aagard_kraken/aagaard_samples/NCS_033_INTRO3_paired_1.fastq  //rdf/Dreycey/aagard_kraken/aagaard_samples/NCS_033_INTRO3_paired_2.fastq
</code></pre>
<p>Transfering this over was filling up all room on the scratch. Turns out the reason why is because:</p>
<pre><code>du -h //rdf/Dreycey/krakenDB
</code></pre>
<pre><code>311M  //rdf/Dreycey/krakenDB/taxonomy
454G  //rdf/Dreycey/krakenDB
</code></pre>
<h2 id="example-run-for-kraken2-using-standard-db">Example run for kraken2 (using standard DB)</h2>
<h3 id="downloading-reads-to-a-metagenomic-sample">Downloading reads to a metagenomic sample</h3>
<h3 id="running-kraken2-command-used">Running Kraken2 (command used)</h3>
<h3 id="output-files">Output files</h3>
<h2 id="databases-used-for-kraken1">Databases used for Kraken1</h2>
<h3 id="off-the-shelve-databases">“Off the shelve” databases</h3>
<ul>
<li>MiniKraken (option #1): 4GB – 99% precision, 65% accuracy</li>
</ul>
<pre><code>wget http://ccb.jhu.edu/software/kraken/dl/minikraken_20171019_4GB.tgz
</code></pre>
<ul>
<li>MiniKraken (option #2): 8GB – 99% precision, 65% accuracy</li>
</ul>
<pre><code>wget http://ccb.jhu.edu/software/kraken/dl/minikraken_20171019_8GB.tgz
</code></pre>
<ul>
<li>Typical full krakenDB: 500GB – 99% precision, 91% accuracy</li>
</ul>
<pre><code>kraken-build --standard --threads 24 --db kraken1_DB --clean
</code></pre>
<p>NOTE: <em>The</em> <strong>" --clean "</strong> <em>switch removes unnecessary intermediate files</em></p>
<h3 id="building-a-custom-kraken1-database">Building a <em>custom</em> kraken1 database</h3>
<h4 id="choosing-libraries">Choosing libraries</h4>
<ul>
<li>Step 1: Create the NCBI taxonomy folder:</li>
</ul>
<pre><code> kraken-build --download-taxonomy --db kraken1_DB
</code></pre>
<ul>
<li>
<p>Step 2: Create a genomic library:<br>
Using the following commands, any of the genomic libraries of interest may be created:</p>
</li>
<li>
<p>Make the library for bacteria (RefSeq)</p>
</li>
</ul>
<pre><code> kraken-build --download-library bacteria --db kraken1_DB
</code></pre>
<ul>
<li>Make the library for archaea (RefSeq)</li>
</ul>
<pre><code> kraken-build --download-library archaea --db kraken1_DB
</code></pre>
<ul>
<li>Make the library for plasmid (RefSeq)</li>
</ul>
<pre><code> kraken-build --download-library plasmid --db kraken1_DB 
</code></pre>
<ul>
<li>Make the library for viral (RefSeq)</li>
</ul>
<pre><code> kraken-build --download-library viral --db kraken1_DB 
</code></pre>
<ul>
<li>Make the library for human (GRCh38 human genome)</li>
</ul>
<pre><code> kraken-build --download-library human --db kraken1_DB 
</code></pre>
<p>From here, it can really depend on what the user is looking for in a sample. Having the size of each of these individual databases would probably be best, however, downloading only the genomic libraries of interest is sure to save space. <strong>Potentially using both the bacterial library and plasmid library in combination with minikraken could be a method to have both broad and comprehensive analysis.</strong></p>
<h4 id="adding-custom-genomes-to-the-database">Adding custom genomes to the database</h4>
<p>The custom files input into the database must be in fasta or multi fasta file format. These must contain NCBI information in the header, or explicitly assigned NCBI information.<br>
(The following is from the manual linked in the header of this file)</p>
<ul>
<li>NCBI information in the header</li>
</ul>
<pre><code> &gt;32630  Adapter sequence
CAAGCAGAAGACGGCATACGAGATCTTCGAGTGACTGGAGTTCCTTGGCACCCGAGAATTCCA
</code></pre>
<ul>
<li>explicitly assigned NCBI information</li>
</ul>
<pre><code> &gt;sequence16|kraken:taxid|32630  Adapter sequence
CAAGCAGAAGACGGCATACGAGATCTTCGAGTGACTGGAGTTCCTTGGCACCCGAGAATTCCA
</code></pre>
<ul>
<li>this is how you add the above sequences to the library:</li>
</ul>
<pre><code>kraken-build --add-to-library fastafile.fa --db kraken1_DB
</code></pre>
<ul>
<li>A list of files in a directory may be added using the following bash command (provided in the kraken 1.0 manual):</li>
</ul>
<pre><code>for file in chr*.fa
do
    kraken-build --add-to-library $file --db kraken1_DB 
done
</code></pre>
<h4 id="final-step-building-the-custom-database">Final step: building the custom database</h4>
<pre><code> kraken-build --build --db $DBNAME --threads 24 --kmer-len 31 --minimizer-len 15 --clean
</code></pre>
<p>Here it may be interesting to deviate to a different kmer or minimizer length, depending on what works the best. (although it is assumed the creators tried many different parameter setting before landing upon these defaults)</p>
<h4 id="building-a-minikraken-database-of-your-own">Building a miniKraken database of your own!!</h4>
<p>You can also shrink a database that you have for Kraken! This effectively allows you to create a minikraken database using an existing larger database.</p>
<p>There are two ways this can be done:<br>
(1) <strong>before</strong> building the larger database<br>
(2) <strong>after</strong> building the larger database</p>
<p>(1) To build the minikrakenDB <strong>before</strong>:</p>
<pre><code> kraken-build --build --db minikraken1_DB --threads 24 --kmer-len 31 --minimizer-len 15 --clean --max-db-size &lt;Max size in GB&gt;
</code></pre>
<p>NOTE: <em>The</em> <strong>" --max-db-size"</strong> <em>switch give the max size for the database in GB</em></p>
<p>(2) To build the minikrakenDB <strong>after</strong>:</p>
<pre><code> kraken-build --shrink 10000 --db kraken1_DB --new-db minikraken
</code></pre>
<p>NOTE: <em>The</em> <strong>" --shrink"</strong> <em>switch give the number of kmers that will be added to the database</em></p>
