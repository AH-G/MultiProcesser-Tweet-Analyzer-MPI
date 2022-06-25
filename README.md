# MultiProcesser Big Data Tweet Analyzer using Python's MPI library.

This repository contains the implementation of the solution of Assignment 1 COMP90024: Cluster and Cloud Computing studied at the University of Melbourne. In this assignment, we were required to develop an efficient multiprocessor based solution to analyze around 5 million tweets to identify the dominant ethnicity of people in different areas of Melbourne. The assignment's description along with the our assignment's report is provided within the repository. All the benchmarks mentioned within the report are performed on University of Melbourne's High Performance Computing (HPC) cluster system known as Spartan. This readme file contains summary of the report. 

## Task
The main goal of this assignment is to develop a system that can efficiently process the large bigTwitter.json JSON file (20+ GB file containing the 5 million tweets) in parallel and count the number of tweets as well as the top 10 most frequent languages for each grid in Sydney as defined in the assignment specification. The challenge is to develop a time- and memory-efficient code by utilizing parallel processing techniques.

## Implementation. 
The implementation is done by virtually dividing the file through read pointers and assigning them to child processes. Each child process then use pandas library to batch process the designated section of the tweets and computes the results. The results are then collected at the parent process where they are combined in an appropriate manner.   

## Benchmark results. 

