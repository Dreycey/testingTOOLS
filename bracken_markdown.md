---


---

<h1 id="bracken">Bracken</h1>
<ul>
<li>Author: Dreycey Albin</li>
<li>Date:  05/13/2019</li>
<li>Updates:</li>
<li>PURPOSE</li>
<li>documentation (website): <a href="https://github.com/jenniferlu717/Bracken">Bracken Github</a></li>
<li>documentation (publication): <a href="https://peerj.com/articles/cs-104/">Bracken publication</a></li>
</ul>
<h2 id="installation-of-bracken">Installation of Bracken</h2>
<ul>
<li>How to install Bracken and build from source:</li>
</ul>
<pre><code>git clone https://github.com/jenniferlu717/Bracken
cd bracken
chmod u+x install_bracken.sh
./install_bracken.sh 
</code></pre>
<p>OR</p>
<ul>
<li>get Bracken from conda:</li>
</ul>
<pre><code>conda install bracken
</code></pre>
<h2 id="building-the-braken-database">Building the Braken database</h2>
<ul>
<li>NOTE: The kmer length is 31 for Kraken and 35 for Kraken 2.0</li>
<li>NOTE: The read length will have to change depending on sequencing read length</li>
<li>GENERAL --&gt; bracken-build -d ${KRAKEN_DB} -t ${THREADS} -k ${KMER_LEN} -l ${READ_LEN}</li>
</ul>
<pre><code>./bracken-build  -k 35 -l 100 -t 40 -d ../../kraken2/Kraken2DB/
</code></pre>
<p>will get the following output:</p>
<pre><code>PROGRAM END TIME: 05-14-2019 15:31:34
Finished creating database100mers.kraken and database100mers.kmer_distrib [in DB folder]
*NOTE: to create read distribution files for multiple read lengths,
rerun this script specifying the same database but a different read length
Bracken build complete.
</code></pre>
<h2 id="running-bracken">Running Bracken</h2>
<pre><code>./bracken -d ../../kraken2/Kraken2DB/ -i ../../kraken2/kraken2_report.report -r 100 -l S -t 22 -o dreyout_bracken
</code></pre>
<ul>
<li>The output looks like the following:</li>
</ul>
<pre><code>name  taxonomy_id taxonomy_lvl  kraken_assigned_reads added_reads new_est_reads fraction_total_reads
Bradyrhizobium erythrophlei 1437360 S 140 23  163 0.01464
Bradyrhizobium sp. SK17 2057741 S 35  61  96  0.00863
Bradyrhizobium icense 1274631 S 35  11  46  0.00419
Bradyrhizobium lablabi  722472  S 34  119 153 0.01373
Bradyrhizobium sp. BTAi1  288000  S 24  8 32  0.00293
Bradyrhizobium japonicum  375 S 24  50  74  0.00662
Bradyrhizobium diazoefficiens 1355477 S 22  19  41  0.00367
Bradyrhizobium sp. CCBAU 51670  1325095 S 22  20  42  0.00377
Rhodopseudomonas palustris  1076  S 100 95  195 0.01751
Rhizobium leguminosarum 384 S 38  70  108 0.00974
Agrobacterium tumefaciens 358 S 26  39  65  0.00587
Methylobacterium sp. 4-46 426117  S 23  35  58  0.00527
Rhodoplanes sp. Z2-YC6860 674703  S 98  26  124 0.01116
Pseudolabrys taiwanensis  331696  S 39  20  59  0.00528
Pseudolabrys sp. FHR47  2562284 S 26  15  41  0.00374
Pseudorhodoplanes sinuspersici  1235591 S 27  6 33  0.00295
Pseudomonas fluorescens 294 S 33  128 161 0.01440
Escherichia coli  562 S 85  1740  1825  0.16319
Klebsiella pneumoniae 573 S 31  316 347 0.03102
Pantoea ananatis  553 S 31  2 33  0.00299
Burkholderia cenocepacia  95486 S 25  158 183 0.01639
Cupriavidus taiwanensis 164546  S 36  63  99  0.00887
Achromobacter xylosoxidans  85698 S 28  62  90  0.00812
Sorangium cellulosum  56  S 79  5 84  0.00755
Streptomyces lydicus  47763 S 29  409 438 0.03917
Streptomyces venezuelae 54571 S 22  150 172 0.01537
Actinoplanes friuliensis  196914  S 22  84  106 0.00953
Nonomuraea sp. ATCC 55076 1909395 S 26  101 127 0.01140
Frankia inefficax 298654  S 25  75  100 0.00900
Planctomyces sp. SH-PL62  1636152 S 26  4 30  0.00272
Paludisphaera borealis  1387353 S 25  3 28  0.00256
Gemmata obscuriglobus 114 S 23  2 25  0.00224
Candidatus Solibacter usitatus  332163  S 26  0 26  0.00232
Homo sapiens  9606  S 5611  357 5968  0.53345
</code></pre>
<h3 id="i-had-a-problem-occur-with-bracken-finding-the-kraken2-fixed----read-bottom">I had a problem occur with Bracken finding the Kraken2 [Fixed – read bottom]</h3>
<p>When running build:</p>
<pre><code>./bracken-build  -k 35  -t 40 -d ../../kraken2/Kraken2DB/
</code></pre>
<p>I got the following output:</p>
<pre><code>&gt;&gt; Selected Options:
kmer length = 35
read length = 100
database  = ../../kraken2/Kraken2DB/
threads = 40
&gt;&gt; Checking for Valid Options...
&gt;&gt; Creating database.kraken [if not found]
database.kraken exists, skipping creation....
Finished creating database.kraken [in DB folder]
&gt;&gt; Creating database100mers.kmer_distrib
&gt;&gt;STEP 0: PARSING COMMAND LINE ARGUMENTS
Taxonomy nodes file:
../../kraken2/Kraken2DB/taxonomy/nodes.dmp
Seqid file:  ../../kraken2/Kraken2DB/seqid2taxid.map
Num Threads: 40
Kmer Length: 35
Read Length: 100
&gt;&gt;STEP 1: READING SEQID2TAXID MAP
49081 total sequences read
&gt;&gt;STEP 2: READING NODES.DMP FILE
2110551 total nodes read
&gt;&gt;STEP 3: READING DATABASE.KRAKEN FILE
0 total sequences read
&gt;&gt;STEP 4: CONVERTING KMER MAPPINGS INTO READ CLASSIFICATIONS:
100mers, with a database built using 35mers
0 sequences converted...
Time Elaped: 0 minutes, 1 seconds, 0.00000 microseconds
=============================
PROGRAM START TIME: 05-14-2019 14:39:27
Traceback (most recent call last):
File "/home/da39/Dreycey/DHS_testing_programs/BRACKEN/Bracken/src/generate_kmer_distribution.py", line 161, in &lt;module&gt;
main()
File "/home/da39/Dreycey/DHS_testing_programs/BRACKEN/Bracken/src/generate_kmer_distribution.py", line 110, in main
i_file = open(args.input, 'r')
FileNotFoundError: [Errno 2] No such file or directory: '../../kraken2/Kraken2DB/database100mers.kraken'
</code></pre>
<p>Turns out the answer is deleting the deleting the<br>
Kraken2DB/database.kraken file:</p>
<pre><code>cd Kraken2DB/;
rm database.kraken;
</code></pre>
<p>credit: <a href="https://github.com/jenniferlu717/Bracken/issues/69">https://github.com/jenniferlu717/Bracken/issues/69</a></p>
<h2 id="parameters">Parameters:</h2>
<pre><code>Usage: bracken -d MY_DB -i INPUT -o OUTPUT -r READ_LEN -l LEVEL -t THRESHOLD
MY_DB  location of Kraken database
INPUT  Kraken REPORT file to use for abundance estimation
OUTPUT file name for Bracken default output
READ_LEN read length to get all classifications for (default: 100)
LEVEL  level to estimate abundance at [options: D,P,C,O,F,G,S] (default: S)
THRESHOLD  number of reads required PRIOR to abundance estimation to perform reestimation (default: 0
</code></pre>
<h2 id="bracken-for-snakemakesingularity">Bracken for Snakemake&amp;Singularity</h2>
<h3 id="install-docker-container">Install docker container</h3>
<p>singularity pull docker://quay.io/biocontainers/bracken:2.2–py27h2d50403_1;</p>
<h4 id="installing-the-minikraken-db-for-both-kraken1-and-kraken2">Installing the minikraken DB (for both kraken1 and kraken2)</h4>
<h5 id="kraken-1.0">Kraken 1.0</h5>
<pre><code>wget https://ccb.jhu.edu/software/kraken/dl/minikraken_20171019_8GB.tgz;
tar -zxvf minikraken_20171019_8GB.tgz;
mv  minikraken_20171019_8GB kraken1DB;
rm minikraken_20171019_8GB.tgz;
</code></pre>
<h5 id="kraken-2.0">Kraken 2.0</h5>
<pre><code>wget ftp://ftp.ccb.jhu.edu/pub/data/kraken2_dbs/minikraken2_v2_8GB_201904_UPDATE.tgz;
tar -zxvf minikraken2_v2_8GB_201904_UPDATE.tgz;
mv  minikraken2_v2_8GB_201904_UPDATE kraken2DB;
rm minikraken2_v2_8GB_201904_UPDATE.tgz;
</code></pre>
<h4 id="installing-the-required-bracken-files-only-needed-for-minikraken-or-else-these-are-built">Installing the required Bracken files (only needed for minikraken or else these are built)</h4>
<p>Download Kraken2 DB files for Bracken:</p>
<pre><code>wget https://ccb.jhu.edu/software/bracken/dl/minikraken2_v2/database100mers.kmer_distrib;
wget https://ccb.jhu.edu/software/bracken/dl/minikraken2_v2/database150mers.kmer_distrib;
wget https://ccb.jhu.edu/software/bracken/dl/minikraken2_v2/database200mers.kmer_distrib;
mv database* kraken2DB/;
</code></pre>
<p>Download Kraken1 DB files for Bracken:</p>
<pre><code>wget https://ccb.jhu.edu/software/bracken/dl/minikraken_8GB_100mers_distrib.txt;
wget https://ccb.jhu.edu/software/bracken/dl/minikraken_8GB_125mers_distrib.txt;
wget https://ccb.jhu.edu/software/bracken/dl/minikraken_8GB_200mers_distrib.txt;
mv minikraken_8GB_* kraken1DB/;
</code></pre>
<h3 id="running-bracken-1">Running Bracken</h3>
<p>mkdir brackenoutput;</p>
<h4 id="we-will-use-the-general-command-below">We will use the general command below:</h4>
<p>singularity exec -B ${PWD}:/tmp/ bracken-2.2–py27h2d50403_1.simg bracken -d  &lt;path to kraken1DB. -i &lt;path to kraken1 report file. -r  -l &lt;map hits to what taxonomy level? S is for species&gt; -t  -o  ;</p>
<h4 id="here-is-the-command-for-running-the-shakya-dataset">Here is the command for running the Shakya dataset:</h4>
<p>export SINGULARITY_BINDPATH=${PWD}/:/tmp;</p>
<p>Using the kraken 1.0 report file:</p>
<pre><code>singularity exec -B ${PWD}:/tmp/ bracken-2.2--py27h2d50403_1.simg bracken -d /tmp/kraken1DB/ -i /tmp/kraken1out.report -r 100 -l S -t 0 -o /tmp/brackenoutput/dreyout_bracken_kraken1;
</code></pre>
<p>Using the kraken 2.0 report file:</p>
<pre><code>singularity exec -B ${PWD}:/tmp/ bracken-2.2--py27h2d50403_1.simg bracken -d /tmp/kraken2DB/ -i /tmp/kraken2out.report -r 100 -l S -t 0 -o /tmp/brackenoutput/dreyout_bracken_kraken2;
</code></pre>
<h2 id="output-files">Output files:</h2>
<pre><code>brackenoutput/dreyout_bracken_kraken1 (79 lines long)
brackenoutput/dreyout_bracken_kraken2 (724 lines long)
</code></pre>
<h2 id="parameters-1">Parameters</h2>
<pre><code>Usage: bracken -d MY_DB -i INPUT -o OUTPUT -r READ_LEN -l LEVEL -t THRESHOLD
MY_DB  location of Kraken database
INPUT  Kraken REPORT file to use for abundance estimation
OUTPUT file name for Bracken default output
READ_LEN read length to get all classifications for (default: 100)
LEVEL  level to estimate abundance at [options: D,P,C,O,F,G,S] (default: S)
THRESHOLD  number of reads required PRIOR to abundance estimation to perform reestimation (default: 0)
</code></pre>

