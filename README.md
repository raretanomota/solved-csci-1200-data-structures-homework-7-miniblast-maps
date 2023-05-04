Download Link: https://assignmentchef.com/product/solved-csci-1200-data-structures-homework-7-miniblast-maps
<br>
BLAST (<strong>B</strong>asic <strong>L</strong>ocal <strong>A</strong>lignment <strong>S</strong>earch <strong>T</strong>ool) is an algorithm for fast, efficient searching of databases of proteins, DNA, and RNA sequences. A BLAST search allows a researcher to compare a query sequence against a library of sequences. For example, after sequencing a gene from a newly discovered bacteria, BLAST can be used to determine if the gene is similar to any genes in known bacteria. You can read more about BLAST at <a href="https://en.wikipedia.org/wiki/BLAST">https://en.wikipedia.org/wiki/BLAST</a> or try it out at <a href="https://blast.ncbi.nlm.nih.gov/Blast.cgi">http://blast.ncbi.nlm.nih. </a><a href="https://blast.ncbi.nlm.nih.gov/Blast.cgi">gov/Blast.cgi</a><a href="https://blast.ncbi.nlm.nih.gov/Blast.cgi">.</a>

In this homework assignment, we create an extremely simplified version of BLAST. Our version works in a manner somewhat similar to BLAST, but the BLAST algorithm is sophisticated and BLAST software is highly optimized. BLAST is used to regularly search Genbank, a repository of sequence data containing over 1 trillion bases (letters) of assembled sequence data. Online searches are typically returned in less than a minute. We won’t try anything quite so ambitious. Our version of BLAST, miniblast, will search small genome files with query strings in a DNA alphabet (A,C,G,T).

The genome files that we will search will consist of a number of lines, each line containing letters from the DNA alphabet. Here are the first few lines of the <em>genome </em><em>small.txt </em>file:

$ head genome_small.txt

TAATGACCTAAATAATCTAAACAAAAGGAAGAGAGATAGTCCGGATTACC

TGGGACATGGAAAACCCTCCTTTCTCTCATCAGCTTCCCACCCCACCTCT

GCCCAGCGCTAATCATGATTTAATAGCCTTCCTTAATCACTTACTCTGTT

TGCTGCTTCATCTAAAAACTTAAGATGCTCTGGGTTAGATCACAGTCTAA

CTCATCACAAATGGATAGAACGACCTGGTAGTTTTCCAGATTTCCATTGT

CCAAACTAATCAGCCAACACACTCAATGATGCACATTATTTTCCACGTAT

ATGGCCTTAGAGATGGGACTAAAAGTCCCGACTGTACTGAGGATGTTTGA

CAGGTTTTGCCATTCTAACTGCTAGTGCTGTGTAATGTGGCTAGGAAGAA

GCAAGGAACAGAGAGATACAGATATGATTTCCTGGGACCAGCTATAGGAG

AGATTCTTCAATTACATCATCTTTGCTCATCCCAAACACCTTGACAAGTA

The genome data above is a small region from human chromosome 18.

The queries will be also strings from the DNA alphabet. A typical query could look like:

CTCATCACAAATGGATAGAACGACCTGGTA

This query can be located at the start of the 5th line.

The strategy that we will employ is to index the genome file with a series of <em>k-mers</em>. A <em>k-mer </em>is a sequence of <em>k </em>letters from the DNA alphabet, where <em>k </em>is an integer.

We index the genome by building an <strong>std::map </strong>with the <em>k-mers </em>as the key and the <em>k-mer </em>positions as values. We build the index by iterating through the genome sequence with a series of overlapping windows of length <em>k</em>. That is, the first <em>k-mer </em>is the genome sequence from 0 to k-1; the second is the sequence from 1 to k, etc. When indexing, we have to allow for the fact that the <em>k-mer </em>may appear multiple times in the genome.

When searching a biological sequence database, it’s often the case that we do not find an exact match to a query string. Miniblast will process queries of varying lengths and allow for mismatches between the genome and query string. To search the genome, miniblast uses the first <em>k </em>letters of the query string as a seed. It is important that searching the database for the initial seed be efficient. We require that the database can be searched in <em>O(log L) </em>time, where <em>L </em>is the number of keys in the database. If the seed can be found in the index, the program should try to extend the match by adding letters from the indexed genomic position until the full query is matched or the allowed number of mismatches is exceeded. For simplicity we require that the seed be an exact match. The mismatches may occur anywhere after the seed in match string.

<em>Please carefully read the entire assignment before beginning your implementation.</em>

<h1>Input/Output &amp; Basic Functionality</h1>

The program should read a series of commands from STDIN and write responses to STDOUT. Sample input and output files have been provided. You can redirect the input and output to your proram using the instructions in the section <strong>Redirecting Input &amp; Output </strong>found at <a href="http://www.cs.rpi.edu/academics/courses/fall15/csci1200/other_information.php">http://www.cs.rpi.edu/academics/ </a><a href="http://www.cs.rpi.edu/academics/courses/fall15/csci1200/other_information.php">courses/fall15/csci1200/other_information.php</a>

Your program should accept the following commands:

<ul>

 <li><em>genome filename </em>– Read a genome sequence from <em>filename</em>. The genome file consists of lines DNA characters.</li>

 <li><em>kmer k </em>– <em>k </em>is an integer. The command will index the genome sequence with <em>k-mers </em>of length <em>k</em>.</li>

 <li><em>query m query </em><em>string </em>– Search the genome for <em>query string </em>allowing for <em>m </em></li>

 <li><em>quit </em>– Exit the program.</li>

</ul>

Here is some sample input, showing the commands:

genome genome_small.txt kmer 10 query 2 TATTACTGCCATTTTGCAGATAAGAAATCAGAAGCTC query 2 TTGACCTTTGGTTAACCCCTCCCTTGAAGGTGAAGCTTGTAAA query 2 AAACACACTGTTTCTAATTCAGGAGGTCTGAGAAGGGA query 2 TCTTGTACTTATTCTCCAATTCAGTCACAGGCCTTGTGGGCTACCCTTCA query 5 TTTTTTTTTTTTTTTCTTTTTT quit

For output, the program will report the query, and if matches are found, the genome position (positions start at 0) of the match, the number of mismatches between the genome and the query string, and the genome sequence matching the query. If a match can’t be found, the program will report “No Match”.

The corresponding output to the input above should be:

Query: TATTACTGCCATTTTGCAGATAAGAAATCAGAAGCTC

504 0 TATTACTGCCATTTTGCAGATAAGAAATCAGAAGCTC

Query: TTGACCTTTGGTTAACCCCTCCCTTGAAGGTGAAGCTTGTAAA

5002 2 TTGACCTTTGGTTAACCAATCCCTTGAAGGTGAAGCTTGTAAA

Query: AAACACACTGTTTCTAATTCAGGAGGTCTGAGAAGGGA

4372 0 AAACACACTGTTTCTAATTCAGGAGGTCTGAGAAGGGA

Query: TCTTGTACTTATTCTCCAATTCAGTCACAGGCCTTGTGGGCTACCCTTCA

No Match

Query: TTTTTTTTTTTTTTTCTTTTTT

<ul>

 <li>0 TTTTTTTTTTTTTTTCTTTTTT</li>

 <li>3 TTTTTTTTTTTTTTCTTTTTTG</li>

 <li>4 TTTTTTTTTTTTTCTTTTTTGA</li>

 <li>5 TTTTTTTTTTTTCTTTTTTGAG</li>

</ul>

2

You are not explicitly required to create any new classes when completing this assignment, but please do so if it will improve your program design. We expect you to use const and pass by reference/alias as appropriate throughout your assignment.

<h1>Order Notation</h1>

In your README.txt file, report the time and space order of your implementation for building the index for a genome of length <strong>L</strong>. Does the <em>k-mer </em>size, <strong>k</strong>, and the average number of locations, <strong>p</strong>, where the key is found affect your answer? What is the order time notation for matching a query of length <strong>q </strong>in a genome of length <strong>L </strong>when the key size is <strong>k </strong>and the key is found at <strong>p </strong>different genomic positions.

<h1>Extra Credit</h1>

Add a new command to implement the database using one of the other data structures that we have covered so far in the course: vectors, lists, arrays etc. Compare the performance your alternative method to the homework method by making a table of run times for each of the genomes and query sets provided with the homework and compare the times to build the index and the times to process the queries. Document any new commands you have added in your README file.


