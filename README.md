# MultiProcesser Parallelized Big Data Tweets Analyzer using Python's MPI library for University of Melbourne's HPC system, Spartan. 

This repository contains the implementation of the solution of Assignment 1 COMP90024: Cluster and Cloud Computing studied at the University of Melbourne. In this assignment, we were required to develop an efficient multiprocessor based solution to analyze around 5 million tweets to identify the dominant ethnicity of people in different areas of Sydney. The assignment's description along with the our assignment's report is provided within the repository. All the benchmarks mentioned within the report are performed on University of Melbourne's High Performance Computing (HPC) cluster system known as Spartan. This readme file contains summary of the report. 

## Scenario
The city of Sydney is divided into 16 grid boxes for the purpose of this assignment. The sydGrid.json file includes the latitudes and longitudes of a range of gridded boxes as illustrated in the figure below, i.e., the latitude and longitude of each of the corners of the boxes is given in the file.

![image](https://user-images.githubusercontent.com/12232515/175768287-2fa567b0-c415-4e65-bb1e-c7983e750b7c.png)

The assignment's objective is to (eventually!) search the large Twitter data set (bigTwitter.json) and using the language used when tweeting, the number of tweets in those languages and the tweet location (lat/long) count the total number of tweets in a given cell that are made in different languages. The final result will be a score for each cell with the following format, where the numbers are obviously representative.

![image](https://user-images.githubusercontent.com/12232515/175768322-e0b3ea9a-f3d8-409e-95ae-86b7c2f961d1.png)

Here cell A1 has 11,111 tweets in total with 11 different languages used for tweets with the most popular being English (9,000 tweets), Chinese (555 tweets), French (444 tweets) with 10th most popular being Greek (66 tweets). Cell A2 has 22 languages used for tweeting with the most popular being English (21,000), Turkish (77 tweets), Swedish (66 tweets) and French being the 10th most popular language (2 tweets). Information on the classification of languages used for tweeting is given in https://developer.twitter.com/en/docs/twitter-for-websites/supported-languages. Simplified Chinese (zh-cn) and Traditional Chinese (zh-tw) are treated as both being Chinese. Tweets with null or undefined (und) for the language attribute should be ignored. Further information on languages that might be used for tweeting is given in https://en.wikipedia.org/wiki/IETF_language_tag. Tweets with no location information can be ignored. Tweets made outside of the Grid can also be ignored.

If a tweet occurs right on the border of two cells, e.g., exactly between the B1/B2 cell border then assume the tweet occurs in B1 (i.e., to the cell on the left). If a tweet occurs exactly on the border between B2/C2 then assume the tweet occurs in C2 (i.e., to the cell below). If a tweet occurs anywhere else on the boundary of a cell, e.g. the upper or leftmost border of A1 then it can be regarded as being in cell A1. Your application should allow a given number of nodes and cores to be utilized. Specifically, your application should be run once to search the bigTwitter.json file on each of the following resources:
- 1 node and 1 core;
- 1 node and 8 cores;
- 2 nodes and 8 cores (with 4 cores per node)

## Task
The main goal of this assignment is to develop a system that can efficiently process the large bigTwitter.json JSON file (20+ GB file containing the 5 million tweets) in parallel and count the number of tweets as well as the top 10 most frequent languages for each grid in Sydney as defined in the assignment specification. The challenge is to develop a time- and memory-efficient code by utilizing parallel processing techniques.

## Implementation. 
The implementation is done by virtually dividing the file through read pointers and assigning them to child processes. Each child process then use pandas library to batch process the designated section of the tweets and computes the results. The results are then collected at the parent process where they are combined in an appropriate manner. The implementation details are much further discussed within the report. 

## Benchmark results. 

We tested our code for 4 different settings of batch processing size (defined earlier): 100, 1000, 10000, 10000 on each of the 3 resource configurations - 1 node with 1 core, 1 node with 8 core, and 2 nodes with 8 core (4 core for each). Figure 1 above shows the execution times (in sec), measured from the start of the script till the end for the root processor, for all 12 runs.

![image](https://user-images.githubusercontent.com/12232515/175774065-fd8012ab-35ae-44f5-b296-851d64ed8d8c.png)

We can easily confirm the effect of parallelization from these results. As expected, we observe worst (slowest) performances for 1 node with 1 core across all batch sizes, ranging from 562 seconds to 1290 seconds. Execution times for 1 node with 8 cores and 2 nodes with 8 cores are generally similar although 1 node with 8 cores consistently outperform the 2 node counterpart by a small amount, which is expected due to possible communication overhead between two nodes.

Furthermore, we identify 1000 as the optimal batch size out of the four candidates, as the execution times for all three resource settings are the smallest compared to those with different batch sizes. While we have only tested a small number of batch sizes, this still provides valuable information in that a sweet-spot (in terms of execution time) in batch size does seem to exist, as we see having unnecessarily small (100) or large (100000) batch sizes result in much slower execution times.
