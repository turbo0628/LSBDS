# LSBDS
###Large Scale Biology Database Search on Xeon Phi platform

LSBDS is developed for searching large-scale biological sequence databases with the Smith-Waterman algorithm on Xeon Phi-based neo-heterogenous architectures. In order to make full use of the compute power of both the multi-core CPU and the many-core Xeon Phi hardware, we use a collaborative computing scheme as well as hybrid parallelism. At the CPU side, we employ SSE intrinsics and multi-threading to implement SIMD parallelism. At the Xeon Phi side, we use Knights Corner vector instructions to gain more data parallelism. Evaluations on real protein sequence databases show that our method achieves a peak performance up to 220 GCUPS on a neo-heterogeneous platform consisting of two Intel E5-2620 CPUs and two Intel Xeon Phi 7110P cards. Our implementation also exhibits good scalability in terms of database size and query length.

##Citation

Our paper is still under submission

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
<pre><code>$./LSDBS -q example_queries/q10-P27895.fasta -db env_nr
</code></pre>

Screen layout:
<pre><code>query path /home/lan/query_seqs/q17-P08519.fasta
db path /home/lan/example_db/env_nr
number of available CPUs: 24
number of available MIC devices: 2
Sequence Database size 1.28GB
top num 10
long query length 4552, pass 7, major length 651, last length 646, average length 650
long query length 4552, pass 7, major length 651, last length 646, average length 650
SSE computing time 25.707000s, AARes 531665920, GCUPs 94.143356
MIC computing time 28.863000s, AARes 429863936, GCUPs 67.794084
MIC computing time 28.896000s, AARes 431134720, GCUPs 67.916848
#Recalc# symbol size 1.86MB
#Recalc# sequence num 296
Recalculator takes 0.24s
total calculation time: 32.087000, GCUPS 194.263434
, total residue 1369360896
----------------top 10 scores---------------
score 630 -- >gi|138634464|gb|ECA95015.1| hypothetical protein GOS_6324106, partial [marine metagenome]
score 538 -- >gi|134394309|gb|EBB10382.1| hypothetical protein GOS_288815, partial [marine metagenome]
score 464 -- >gi|141083129|gb|ECP50619.1| hypothetical protein GOS_5820514, partial [marine metagenome]
score 350 -- >gi|143826383|gb|EDG90597.1| hypothetical protein GOS_700359 [marine metagenome]
score 306 -- >gi|684297128|gb|KGA16158.1| hypothetical protein GM51_13185 [freshwater metagenome]
score 299 -- >gi|138611756|gb|ECA80116.1| hypothetical protein GOS_3417296, partial [marine metagenome]
score 297 -- >gi|816460845|gb|KKK90825.1| hypothetical protein LCGC14_2719120, partial [marine sediment metagenome]
score 287 -- >gi|138400420|gb|EBZ40745.1| hypothetical protein GOS_5461389, partial [marine metagenome]
score 287 -- >gi|144047815|gb|EDI48792.1| hypothetical protein GOS_424353 [marine metagenome]
score 280 -- >gi|136269826|gb|EBN02706.1| hypothetical protein GOS_8313652, partial [marine metagenome]
--------------------------------------------
</code></pre>

##Contacts

Please contact Weiguo,Liu ( weiguo.liu@sdu.edu.cn) for support. We welcome any bug reporting and improvement suggestions.
