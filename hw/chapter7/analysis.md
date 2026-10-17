# Chapter 7: CPU Scheduling

## Q1
When all three jobs have a length of 200, FIFO and SJF produce the same schedule because all jobs have equal lengths. Job 0 starts at time 0, Job 1 starts at time 200, and Job 2 starts at time 400. The response times are 0, 200, and 400, giving an average response time of 200. The turnaround times are 200, 400, and 600, giving an average turnaround time of 400.

## Q2
For job lengths of 100, 200, and 300, FIFO and SJF execute the jobs in the same order because the jobs are already arranged from shortest to longest. The response times are 0, 100, and 300, and the turnaround times are 100, 300, and 600. The average response time is 133.33, and the average turnaround time is 333.33.

## Q3
With Round Robin and a time quantum of 1, for job lengths of 100, 200, and 300, the simulator reports response times of 0, 1, and 2, and turnaround times of 298, 499, and 600. The average response time is 1, while the average turnaround time is 465.67.

## Q4
SJF gives the same turnaround times as FIFO when the jobs arrive in nondecreasing order of their lengths, from shortest to longest. Both policies then execute the jobs in the same order.

## Q5
One workload that makes SJF and Round Robin produce the same response times is a set of equal-length jobs, with the RR time quantum equal to the length of each job. For example, with three jobs of length 10 and an RR quantum of 10, both policies give response times of 0, 10, and 20.

## Q6
Under non-preemptive SJF, the first job has a response time of zero. Each subsequent job must wait for all jobs scheduled before it to finish. Therefore, the response time of a later job is the sum of the execution lengths of the preceding jobs, assuming all jobs arrive at time zero and are ordered from shortest to longest.

## Q7
As the RR time quantum increases, the response time of later jobs generally increases because each preceding job can occupy the CPU for a longer time before the next job receives its first turn. For three jobs of length 10, the expected response times are (0, 1, 2) for q = 1, (0, 2, 4) for q = 2, and (0, 5, 10) for q = 5.

For N jobs that all arrive at time zero, the worst-case response time of the last job is:

T_response,worst = (N - 1) * q

This bound occurs when each of the N - 1 jobs ahead of the last job uses a full time quantum before the last job receives its first CPU turn.
