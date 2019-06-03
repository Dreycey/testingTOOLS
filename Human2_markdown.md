---


---

<h1 id="human2">HuMaN2</h1>
<ul>
<li>Author: Dreycey Albin</li>
<li>Date: 06/03/2019</li>
<li>Updates: NAME (DATE): update descr; NAME (DATE): update descr,</li>
<li>HuMaN2 is a</li>
<li>documentation (website): <a href="http://huttenhower.sph.harvard.edu/humann2">http://huttenhower.sph.harvard.edu/humann2</a></li>
<li>documentation (publication): <a href="https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6235447/">https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6235447/</a></li>
</ul>
<h2 id="installation">Installation</h2>
<p>INSTRUCTIONS ON HOW TO INSTALL USING TERMINAL</p>
<h3 id="installation-for-macos">installation for MacOS</h3>
<pre><code></code></pre>
<h3 id="installation-for-linux">installation for Linux</h3>
<h4 id="download-human2">Download HuMaN2</h4>
<pre><code>wget https://files.pythonhosted.org/packages/f2/c6/01ef12082fa44f06c5329bacb49c6dbeb4a04c66a60b0f64e4a9f1fb824a/humann2-0.11.2.tar.gz;
tar -zxvf humann2-0.11.2.tar.gz;
</code></pre>
<h4 id="install-the-package">install the package</h4>
<pre><code>python setup.py install
</code></pre>
<ul>
<li>One can check if the installation worked by trying:</li>
</ul>
<pre><code>which humann2
</code></pre>
<p>From which a path to the humann2 binary will be printed.</p>
<h2 id="basic-commands">Basic commands</h2>
<h3 id="input">INPUT</h3>
<pre><code>humann2 --input &lt;path_to_fastq_file&gt; --output &lt;path_to_output_directory&gt;
</code></pre>
<h3 id="output">OUTPUT</h3>
<pre><code>output_directory/demo_genefamilies.tsv;
output_directory/demo_pathabundance.tsv;
output_directory/demo_pathcoverage.tsv;
</code></pre>
<ul>
<li>inside demo_genefamilies.tsv</li>
</ul>
<pre><code>head -10 output_directory/demo_genefamilies.tsv
</code></pre>
<pre><code># Gene Family  demo_Abundance-RPKs
UNMAPPED  3897.0000000000
UniRef90_unknown  1633.3754216411
UniRef90_unknown|g__Bacteroides.s__Bacteroides_dorei  894.7763053061
UniRef90_unknown|g__Bacteroides.s__Bacteroides_vulgatus  738.5991163350
UniRef90_R6HHA8  333.3333333333
UniRef90_R6HHA8|g__Bacteroides.s__Bacteroides_dorei  333.3333333333
UniRef90_R7NYS9  166.6666666667
UniRef90_R7NYS9|g__Bacteroides.s__Bacteroides_vulgatus  166.6666666667
UniRef90_D1K9F5  66.6666666667
</code></pre>
<ul>
<li>inside demo_pathabundance.tsv</li>
</ul>
<pre><code>head -10 output_directory/demo_pathabundance.tsv
</code></pre>
<pre><code># Pathway  demo_Abundance
UNMAPPED  1225.9730368639
UNINTEGRATED  5908.3755167593
UNINTEGRATED|g__Bacteroides.s__Bacteroides_dorei  3123.1902040899
UNINTEGRATED|g__Bacteroides.s__Bacteroides_vulgatus  2753.6043067990
UNINTEGRATED|unclassified  72.8101423152
PWY-6305: putrescine biosynthesis IV  30.3913024756
PWY-6305: putrescine biosynthesis IV|unclassified  30.3913024756
PWY-4203: volatile benzenoid biosynthesis I (ester formation)  22.5319052245
PWY-4203: volatile benzenoid biosynthesis I (ester formation)|unclassified  22.5319052245
</code></pre>
<ul>
<li>inside demo_pathcoverage.tsv</li>
</ul>
<pre><code>head -10 output_directory/demo_pathcoverage.tsv
</code></pre>
<pre><code># Pathway  demo_Coverage
UNMAPPED  1.0000000000
UNINTEGRATED  1.0000000000
UNINTEGRATED|g__Bacteroides.s__Bacteroides_dorei  1.0000000000
UNINTEGRATED|g__Bacteroides.s__Bacteroides_vulgatus  1.0000000000
UNINTEGRATED|unclassified  1.0000000000
PWY-6305: putrescine biosynthesis IV  0.9999818077
PWY-6305: putrescine biosynthesis IV|unclassified  0.9401999667
PWY-4203: volatile benzenoid biosynthesis I (ester formation)  0.9992342795
PWY-4203: volatile benzenoid biosynthesis I (ester formation)|unclassified  0.6687522557
</code></pre>
<h2 id="example_1">Example_1</h2>
<h3 id="input-files">Input files</h3>
<ul>
<li>Test out the demo file given in the humann2 download folder</li>
<li>The input file in this case is a fastq reads file.</li>
</ul>
<pre><code>examples/demo.fastq 
</code></pre>
<h3 id="commands">commands</h3>
<pre><code>humann2 --input examples/demo.fastq --output output_directory
</code></pre>
<h3 id="output-files">Output files</h3>
<ul>
<li></li>
</ul>
<pre><code>/scratch/dreycey/DHS_testing_programs/humann2-0.11.2/output_directory/demo_genefamilies.tsv

/scratch/dreycey/DHS_testing_programs/humann2-0.11.2/output_directory/demo_pathabundance.tsv

/scratch/dreycey/DHS_testing_programs/humann2-0.11.2/output_directory/demo_pathcoverage.tsv
</code></pre>
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

