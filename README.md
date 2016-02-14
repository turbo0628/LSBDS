# LSBDS
###Large Scale Biology Database Search on Xeon Phi platform

LSBDS is developed for searching large-scale biological sequence databases with the Smith-Waterman algorithm on Xeon Phi-based neo-heterogenous architectures. In order to make full use of the compute power of both the multi-core CPU and the many-core Xeon Phi hardware, we use a collaborative computing scheme as well as hybrid parallelism. At the CPU side, we employ SSE intrinsics and multi-threading to implement SIMD parallelism. At the Xeon Phi side, we use Knights Corner vector instructions to gain more data parallelism. Evaluations on real protein sequence databases show that our method achieves a peak performance up to 220 GCUPS on a neo-heterogeneous platform consisting of two Intel E5-2620 CPUs and two Intel Xeon Phi 7110P cards. Our implementation also exhibits good scalability in terms of database size and query length.

##Citation

H. Lan, W. Liu*, B. Schmidt, and B. Wang: Accelerating Large-Scale Biological Database Search on Xeon Phi-based Neo-Heterogeneous Architectures, 2015 IEEE International Conference on Bioinformatics and Biomedicine (BIBM 2015), Washington D.C., USA, November 2015.

###Related Papers

XSW: Accelerating Biological Database Search on Xeon Phi
Lipeng Wang, Yuandong Chan, Xiaohui Duan, Haidong Lan, Xiangxu Meng, and Weiguo Liu.
The 28th IEEE International Parallel & Distributed Processing Symposium (IPDPS 2014), Phoenix,USA, May, 2014.

##Usage

We provide binaries compiled with ICC 15.0.0 on CENTOS 6.4. MPSS 3.2.1-1 or later and Composer XE 2015 are recommended to build runtime environment.

The DBmaker is an auxiliary program to carry out the database preprocessing operation. Please process your database file before searching.
This program generates three files with .map .seq .title extensions which stored all the information in the database. You could remove the original FASTA file safely. We'll develop a reverse program for format transformation soon.

<pre><code>./DBmaker [path_to_databaase_file]
</code></pre>
Currently only databases in fasta format are accepted.

The LSDBS search the processed database using both CPUs and Intel Phi cards.  You could specify either of them or their common suffix name in the .
Parameter list:

<pre><code>./LSDBS 
Input Files:
-q <str> (QUERY_FILE) 
-d (DATABASE_FILE)

Scoring Scheme:
-m <str> (Scoring matrix name, default = blosum62, supported)
-g <int> (gap open penalty, default = 10)
-e <int> (gap extend penalty, default = 2)
-v       (verbose, show current search progress and system configuration)
</code></pre>

###Typical Search Example

You could access to the swiss protein database using the following url:

ftp://ftp.uniprot.org/pub/databases/uniprot/current_release/knowledgebase/complete/uniprot_sprot.fasta.gz

This database is quite small and not likely to fully feed the devices.

We use the env_nr database from NCBI as an example:

Firstly download the database
<pre><code>$wget ftp://ftp.ncbi.nlm.nih.gov/blast/db/FASTA/env_nr.gz
</code></pre>

And then unzip the tarball
<pre><code>$gunzip env_nr.gz
</code></pre>

Process the database

<pre><code>$./DBmaker env_nr
</code></pre>
Screen layout:

<pre><code>flushing out chunk 0 of size 256MB
flushing out chunk 1 of size 255MB
flushing out chunk 2 of size 255MB
flushing out chunk 3 of size 255MB
flushing out chunk 4 of size 255MB
flushing out chunk 5 of size 24MB
last batch num 72
total sequence num is 6865992 68c448
closing files
file closed
</code></pre>

Now you can search the database with the example queries.
<pre><code>$./LSDBS -q example_queries/q20-Q9UKN1.fasta -db ./env_nr
</code></pre>

Screen layout:
<pre><code>query path example_queries/q20-Q9UKN1.fasta
db path ./env_nr
number of available CPUs: 24
number of available MIC devices: 2
Sequence Database size 1.28GB
top num 10
long query length 5480, pass 8, major length 685, last length 685, average length 685
long query length 5480, pass 8, major length 685, last length 685, average length 685
MIC computing time 31.164000s, AARes 392385024, GCUPs 68.998522
SSE computing time 33.890000s, AARes 596055552, GCUPs 96.381954
MIC computing time 31.425000s, AARes 395311616, GCUPs 68.935805
#Recalc# symbol size 2.26MB
#Recalc# sequence num 993
Recalculator takes 0.27s
total calculation time: 34.737000, GCUPS 216.026073
, total residue 1369360896
----------------top 10 scores---------------
score 497 -- >gi|816799270|gb|KKN75037.1| hypothetical protein LCGC14_0384110, partial [marine sediment metagenome]
score 373 -- >gi|816760533|gb|KKN41062.1| hypothetical protein LCGC14_0727120 [marine sediment metagenome]
score 357 -- >gi|143302314|gb|EDE20598.1| hypothetical protein GOS_1171242, partial [marine metagenome]
score 353 -- >gi|816543413|gb|KKL55155.1| hypothetical protein LCGC14_2258220, partial [marine sediment metagenome]
score 331 -- >gi|143177023|gb|EDD37053.1| hypothetical protein GOS_1315847, partial [marine metagenome]
score 330 -- >gi|135253791|gb|EBG44713.1| hypothetical protein GOS_9431765, partial [marine metagenome]
score 322 -- >gi|143880528|gb|EDH29922.1| hypothetical protein GOS_630718, partial [marine metagenome]
score 314 -- >gi|143324238|gb|EDE33522.1| hypothetical protein GOS_1148440, partial [marine metagenome]
score 308 -- >gi|136135495|gb|EBM14790.1| hypothetical protein GOS_8455521, partial [marine metagenome]
score 307 -- >gi|143406335|gb|EDE78403.1| hypothetical protein GOS_1070628, partial [marine metagenome]
--------------------------------------------
</code></pre>

##Contacts

Please contact Weiguo,Liu ( weiguo.liu@sdu.edu.cn) for support. We welcome any bug reporting and improvement suggestions.
