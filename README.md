# MultiProcesser Parallelized Big Data Tweets Analyzer using Python's MPI library for University of Melbourne's HPC system, Spartan. 

This repository contains the implementation of the solution of Assignment 1 COMP90024: Cluster and Cloud Computing studied at the University of Melbourne. In this assignment, we were required to develop an efficient multiprocessor based solution to analyze around 5 million tweets to identify the dominant ethnicity of people in different areas of Sydney. The assignment's description along with the our assignment's report is provided within the repository. All the benchmarks mentioned within the report are performed on University of Melbourne's High Performance Computing (HPC) cluster system known as Spartan. This readme file contains summary of the report. 

## Scenario
The sydGrid.json file includes the latitudes and longitudes of a range of gridded boxes as illustrated in the figure below, i.e., the latitude and longitude of each of the corners of the boxes is given in the file.

![image](https://user-images.githubusercontent.com/12232515/175768287-2fa567b0-c415-4e65-bb1e-c7983e750b7c.png)

Your assignment is to (eventually!) search the large Twitter data set (bigTwitter.json) and using the language used when tweeting, the number of tweets in those languages and the tweet location (lat/long) count the total number of tweets in a given cell that are made in different languages. The final result will be a score for each cell with the following format, where the numbers are obviously representative.

![image](https://user-images.githubusercontent.com/12232515/175768322-e0b3ea9a-f3d8-409e-95ae-86b7c2f961d1.png)

Here cell A1 has 11,111 tweets in total with 11 different languages used for tweets with the most popular being English (9,000 tweets), Chinese (555 tweets), French (444 tweets) with 10th most popular being Greek (66 tweets). Cell A2 has 22 languages used for tweeting with the most popular being English (21,000), Turkish (77 tweets), Swedish (66 tweets) and French being the 10th most popular language (2 tweets). Information on the classification of languages used for tweeting is given in https://developer.twitter.com/en/docs/twitter-for-websites/supported-languages. You may treat Simplified Chinese (zh-cn) and Traditional Chinese (zh-tw) as both being Chinese. Tweets with null or undefined (und) for the language attribute can be ignored. Further information on languages that might be used for tweeting is given in https://en.wikipedia.org/wiki/IETF_language_tag. Tweets with no location information can be ignored. Tweets made outside of the Grid can also be ignored.

If a tweet occurs right on the border of two cells, e.g., exactly between the B1/B2 cell border then assume the tweet occurs in B1 (i.e., to the cell on the left). If a tweet occurs exactly on the border between B2/C2 then assume the tweet occurs in C2 (i.e., to the cell below). If a tweet occurs anywhere else on the boundary of a cell, e.g. the upper or leftmost border of A1 then it can be regarded as being in cell A1. Your application should allow a given number of nodes and cores to be utilized. Specifically, your application should be run once to search the bigTwitter.json file on each of the following resources:
- 1 node and 1 core;
- 1 node and 8 cores;
- 2 nodes and 8 cores (with 4 cores per node)

## Task
The main goal of this assignment is to develop a system that can efficiently process the large bigTwitter.json JSON file (20+ GB file containing the 5 million tweets) in parallel and count the number of tweets as well as the top 10 most frequent languages for each grid in Sydney as defined in the assignment specification. The challenge is to develop a time- and memory-efficient code by utilizing parallel processing techniques.

## Implementation. 
The implementation is done by virtually dividing the file through read pointers and assigning them to child processes. Each child process then use pandas library to batch process the designated section of the tweets and computes the results. The results are then collected at the parent process where they are combined in an appropriate manner.   

## Benchmark results. 

