# LSBDS
##Large Scale Biology Database Search on Xeon Phi platform

LSBDS is developed for searching large-scale biological sequence databases with the Smith-Waterman algorithm on Xeon Phi-based neo-heterogenous architectures. In order to make full use of the compute power of both the multi-core CPU and the many-core Xeon Phi hardware, we use a collaborative computing scheme as well as hybrid parallelism. At the CPU side, we employ SSE intrinsics and multi-threading to implement SIMD parallelism. At the Xeon Phi side, we use Knights Corner vector instructions to gain more data parallelism. Evaluations on real protein sequence databases show that our method achieves a peak performance up to 220 GCUPS on a neo-heterogeneous platform consisting of two Intel E5-2620 CPUs and two Intel Xeon Phi 7110P cards. Our implementation also exhibits good scalability in terms of database size and query length.

#Citation

Our paper is still under submission

##Related Papers

XSW: Accelerating Biological Database Search on Xeon Phi
Lipeng Wang, Yuandong Chan, Xiaohui Duan, Haidong Lan, Xiangxu Meng, and Weiguo Liu.
The 28th IEEE International Parallel & Distributed Processing Symposium (IPDPS 2014), Phoenix,USA, May, 2014.

SWAPHI: Smith-Waterman protein database search on Xeon Phi coprocessors
Yongchao Liu and Bertil Schmidt. 25th IEEE International Conference on Application-specific Systems, Architectures and Processors (ASAP 2014), 2014, pp. 184-185.

#Usage

We provide binaries compiled with ICC 15.0.0 on CENTOS 6.4.

The makeDB is an auxiliary program to carry out the database preprocessing operation. Please process your database file before searching.

./makeDB [path_to_databaase_file]

Currently only database in fasta format are accepted.

The LSDBS search the processed database using both CPUs and Intel Phi cards.
The parameter list is followed:

./LSDBS 
Input Files:
-q <str> (QUERY_FILE) 
-d (DATABASE_FILE)

Scoring Scheme:
-m <str> (Scoring matrix name, default = blosum62, supported)
-g <int> (gap open penalty, default = 10)
-e <int> (gap extend penalty, default = 2)

#Contacts

Please contact Weiguo,Liu ( weiguo.liu@sdu.edu.cn) for support. We welcome any bug reporting and improvement suggestions.
