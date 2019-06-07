---


---

<h1 id="metaphlan2">MetaPhlAn2</h1>
<ul>
<li>Author: Dreycey Albin</li>
<li>Date: 06/03/2019</li>
<li>Updates: NAME (DATE): update descr; NAME (DATE): update descr,</li>
<li>MetaPhlAn2 is a classification tool that uses signature sequences to classify bacteria in a sample using sequencing data as input.</li>
<li>documentation (website): <a href="https://bitbucket.org/biobakery/biobakery/wiki/metaphlan2">https://bitbucket.org/biobakery/biobakery/wiki/metaphlan2</a></li>
<li>documentation (publication): URL TO PUBLICATION</li>
</ul>
<h2 id="installation">Installation</h2>
<p>INSTRUCTIONS ON HOW TO INSTALL USING TERMINAL</p>
<ul>
<li>installation for MacOS</li>
</ul>
<pre><code>brew tap biobakery/biobakery;
brew install metaphlan2;
</code></pre>
<ul>
<li>installation for Linux</li>
</ul>
<pre><code>wget https://bitbucket.org/biobakery/metaphlan2/get/default.zip;
unzip default.zip;
</code></pre>
<h2 id="basic-commands">Basic commands</h2>
<h3 id="input">INPUT</h3>
<p>The general input for MetPhlAn is a fasta file with sequences to query.</p>
<p>*General usage:</p>
<pre><code>usage: metaphlan2.py --input_type
{fastq,fasta,multifasta,multifastq,bowtie2out,sam}
[--mpa_pkl MPA_PKL] [--bowtie2db METAPHLAN_BOWTIE2_DB]
[-x INDEX] [--bt2_ps BowTie2 presets]
[--bowtie2_exe BOWTIE2_EXE]
[--bowtie2_build BOWTIE2_BUILD] [--bowtie2out FILE_NAME]
[--no_map] [--tmp_dir] [--tax_lev TAXONOMIC_LEVEL]
[--min_cu_len] [--min_alignment_len] [--ignore_viruses]
[--ignore_eukaryotes] [--ignore_bacteria]
[--ignore_archaea] [--stat_q]
[--ignore_markers IGNORE_MARKERS] [--avoid_disqm]
[--stat] [-t ANALYSIS TYPE] [--nreads NUMBER_OF_READS]
[--pres_th PRESENCE_THRESHOLD] [--clade] [--min_ab]
[-o output file] [--sample_id_key name]
[--sample_id value] [-s sam_output_file]
[--biom biom_output] [--mdelim mdelim] [--nproc N]
[--install] [--read_min_len READ_MIN_LEN] [-v] [-h]
[INPUT_FILE] [OUTPUT_FILE]
</code></pre>
<h3 id="output">OUTPUT</h3>
<pre><code>
</code></pre>
<h2 id="example_1">Example_1</h2>
<h3 id="input-files">Input files</h3>
<ul>
<li>First download some of the data</li>
</ul>
<pre><code>wget https://bitbucket.org/biobakery/biobakery/raw/tip/demos/biobakery_demos/data/metaphlan2/input/SRS014476-Supragingival_plaque.fasta.gz;
wget https://bitbucket.org/biobakery/biobakery/raw/tip/demos/biobakery_demos/data/metaphlan2/input/SRS014494-Posterior_fornix.fasta.gz;
wget https://bitbucket.org/biobakery/biobakery/raw/tip/demos/biobakery_demos/data/metaphlan2/input/SRS014459-Stool.fasta.gz;
wget https://bitbucket.org/biobakery/biobakery/raw/tip/demos/biobakery_demos/data/metaphlan2/input/SRS014464-Anterior_nares.fasta.gz;
wget https://bitbucket.org/biobakery/biobakery/raw/tip/demos/biobakery_demos/data/metaphlan2/input/SRS014470-Tongue_dorsum.fasta.gz;
wget https://bitbucket.org/biobakery/biobakery/raw/tip/demos/biobakery_demos/data/metaphlan2/input/SRS014472-Buccal_mucosa.fasta.gz;
mkdir demo_1;
mv SRS* demo_1/;
cd demo_1;
c
</code></pre>
<h3 id="commands">commands</h3>
<pre><code>python2 ../metaphlan2.py SRS014476-Supragingival_plaque.fasta.gz  --input_type fasta &gt; SRS014476-Supragingival_plaque_profile.txt;
python2 ../metaphlan2.py SRS014494-Posterior_fornix.fasta.gz  --input_type fasta &gt; SRS014494-Posterior_fornix.fasta.txt;
python2 ../metaphlan2.py SRS014459-Stool.fasta.gz  --input_type fasta &gt; SRS014459-Stool.fasta.txt;
python2 ../metaphlan2.py SRS014464-Anterior_nares.fasta.gz  --input_type fasta &gt; SRS014464-Anterior_nares.fasta.txt;
python2 ../metaphlan2.py SRS014470-Tongue_dorsum.fasta.gz  --input_type fasta &gt; SRS014470-Tongue_dorsum.fasta.txt;
python2 ../metaphlan2.py SRS014472-Buccal_mucosa.fasta.gz  --input_type fasta &gt; SRS014472-Buccal_mucosa.fasta.txt;
`
</code></pre>
<h3 id="output-files">Output files</h3>
<ul>
<li></li>
</ul>
<pre><code></code></pre>
<h2 id="example-2">Example 2:</h2>
<h3 id="input-files-1">Input files</h3>
<pre><code></code></pre>
<h3 id="output-files-1">Output files</h3>
<ul>
<li></li>
</ul>
<pre><code></code></pre>
<h3 id="commands-1">commands</h3>
<ul>
<li></li>
</ul>
<pre><code></code></pre>

