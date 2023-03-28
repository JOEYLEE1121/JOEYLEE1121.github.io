---
layout: single
title: "Sample Sort with MultiThreading in C"
categories: "school-project"
tag: [algorithm, C]
toc: true
toc_sticky: true
---

# Faster quick sort using multithreading

Score: 8.5 / 10.0

## Task

Design a multithreading algorithm that runs faster than normal quick sort.


## Code

```c
// COMP3230 Programming Assignment Two
// The sequential version of the sorting using qsort

/*
# Filename: psort_3035729629
# Student name and No.: Chang Jun Lee 3035729629
# Development platform: Docker
# Remark: In order to test with different worker thread number, please follow the instruction below:

    1. change global variable THREAD_NUM in line 27 to intended worker thread number
    2. save and run the command: gcc psort_303572969.c -o psort_303572969 -pthread
    3. run the command: ./psort_303572969 <size> <worker thread number>
    4. if the input of number of worker thread is different, the program will exit with instruction.
*/

#include <stdio.h>
#include <stdlib.h>
#include <pthread.h>
#include <errno.h>
#include <sys/time.h>
#include <time.h>
#include <unistd.h>

int checking(unsigned int *, long);
int compare(const void *, const void *);

// global variables
#define THREAD_NUM 16 // number of threads <- change this value manually to test for different number of worker threads!

long size; // size of the array
int thread_num; // user argc[2] will be stored here. Check whether this value equals THREAD_NUM will be processed.
unsigned int *intarr; // array of random integers
int callcount = 0;    // for counting merge function
int thread_merge_count = 0;
int merged[THREAD_NUM * THREAD_NUM];
int pivot[THREAD_NUM - 1]; // stores pivot index in phase 2
int thread_id = -1;
int new_partition_length[THREAD_NUM]; // stores partition lengths after merge in phase 4
int **shared2darray;     // for partitions merging between threads
int flag[THREAD_NUM][1]; // for keeping track of memory allocation of shared2darray
int partition_merge_callcount = 0;
int partition_sort_callcount = 0;
int partition_sort_completecount = 0;

pthread_mutex_t mutex = PTHREAD_MUTEX_INITIALIZER;
pthread_mutex_t mutex2 = PTHREAD_MUTEX_INITIALIZER;
pthread_mutex_t mutex3 = PTHREAD_MUTEX_INITIALIZER;
pthread_mutex_t mutex4 = PTHREAD_MUTEX_INITIALIZER;
pthread_mutex_t mutex5 = PTHREAD_MUTEX_INITIALIZER;
pthread_mutex_t mutex6 = PTHREAD_MUTEX_INITIALIZER;

// functions for sorting

int threadIDAssign() // thread id will be assigned. For example, if THREAD_NUM = 4, there will be 4 threads with id 0,1,2,3
{
    pthread_mutex_lock(&mutex2);
    thread_id++;
    pthread_mutex_unlock(&mutex2);
    return thread_id;
}

void *threadPartitionMergeAndSort(int id, int *partitions_length, int **partition_2darray) // 
{

    pthread_mutex_lock(&mutex4);
    for (int i = 0; i < THREAD_NUM; i++)
    {
        new_partition_length[i] += partitions_length[i];

        if (thread_merge_count == THREAD_NUM - 1)
        {
            shared2darray[i] = malloc(sizeof(int *) * new_partition_length[i]);
        }
    }
    pthread_mutex_unlock(&mutex4);

    thread_merge_count++;

    pthread_mutex_lock(&mutex6);

    while (1) // handles phase 4, sequences are allocated to 2d array.
    {
        if (thread_merge_count == THREAD_NUM)
        {

            int starting_point = 0;

            for (int i = 0; i < id; i++)
            {
                starting_point += new_partition_length[i];
            }

            pthread_mutex_lock(&mutex5);

            partition_merge_callcount++;

            for (int i = 0; i < THREAD_NUM; i++)
            {
                for (int j = 0; j < partitions_length[i]; j++)
                {
                    shared2darray[i][flag[i][0]] = partition_2darray[i][j];
                    flag[i][0]++;
                }
            }

            pthread_mutex_unlock(&mutex5);

            break;
        }
    }
    pthread_mutex_unlock(&mutex6);

    while (1)
    {
        if (partition_merge_callcount == THREAD_NUM)
        {
            qsort(shared2darray[id], new_partition_length[id], sizeof(unsigned int), compare); // sort in phase 5
            partition_sort_callcount++;
            break;
        }
    }

    while (1)
    {
        if (partition_sort_callcount == THREAD_NUM)
        {
            for (int i = 0; i < THREAD_NUM; i++)
            {
                for (int j = 0; j < new_partition_length[i]; j++)
                {
                }
            }
            partition_sort_completecount++;
            break;
        }
    }

    while (1)
    {
        if (partition_sort_completecount == THREAD_NUM)
        {
            int count = 0;
            for (int i = 0; i < THREAD_NUM; i++)
            {
                for (int j = 0; j < new_partition_length[i]; j++)
                {
                    intarr[count] = shared2darray[i][j]; // latest thread will overwrite intarr with the sorted array
                    count++;
                }
            }
            break;
        }
    }
}

void *mergeAndSortSampleandSelectPivot(int *sample)
{

    for (int i = 0; i < THREAD_NUM; i++)
    {
        merged[callcount + i] = sample[i];
    }
    callcount = callcount + THREAD_NUM;

    if (callcount == THREAD_NUM * THREAD_NUM)
    {

        qsort(merged, THREAD_NUM * THREAD_NUM, sizeof(unsigned int), compare);

        for (int i = 0; i < THREAD_NUM - 1; i++) // select pivot
        {
            pivot[i] = merged[(i + 1) * THREAD_NUM + (THREAD_NUM / 2) - 1];
        }
        callcount++;
    }
}

void *sortInThreads(void *argc) // threads will get size / THREAD_NUM elements first (except for the last thread)
{

    int current_thread_id = threadIDAssign();

    int *array_head = (int *)argc;
    int seclength = size / THREAD_NUM;

    int *temp = malloc(sizeof(int) * (seclength)); // temporary array to store the input elements
    for (int k = 0; k < (seclength); k++)
    {
        temp[k] = *array_head;
        array_head++;
    }

    qsort(temp, seclength, sizeof(unsigned int), compare); // phase 1 sort

    int *sample = malloc(sizeof(int) * THREAD_NUM);

    for (int i = 0; i < THREAD_NUM; i++) // create sample array in phase 1
    {
        sample[i] = temp[i * size / (THREAD_NUM * THREAD_NUM)];
    }

    pthread_mutex_lock(&mutex);
    mergeAndSortSampleandSelectPivot(sample); // completes pahse 2
    pthread_mutex_unlock(&mutex);

    while (1)
    {
        if (callcount == THREAD_NUM * THREAD_NUM + 1) // continue only after phase 2 is done
        {

            int *subseq = malloc(sizeof(int) * (THREAD_NUM));
            subseq[0] = 0;
            int subseqidx = 1;

            for (int i = 0; i < THREAD_NUM; i++) // set subsequences length by pivots 
            {
                for (int j = 0; j < seclength; j++)
                {
                    if (temp[j] > pivot[i])
                    {
                        subseq[subseqidx] = j;
                        subseqidx++;
                        j = j;
                        break;
                    }
                }
            }

            int **array2d = malloc(sizeof(int *) * THREAD_NUM);
            int *secseglength = malloc(sizeof(int *) * THREAD_NUM);

            // define partition lengths
            for (int i = 0; i < THREAD_NUM - 1; i++)
            {
                secseglength[i] = subseq[i + 1] - subseq[i];
            }
            secseglength[THREAD_NUM - 1] = seclength - subseq[THREAD_NUM - 1];

            // allocate 2d array length accrodingly
            for (int i = 0; i < THREAD_NUM; i++)
            {
                array2d[i] = malloc(sizeof(int *) * secseglength[i]);
            }

            int count = 0;
            for (int i = 0; i < THREAD_NUM; i++)
            {
                for (int j = 0; j < secseglength[i]; j++) //completes phase 3
                {
                    array2d[i][j] = temp[count];
                    count++;
                }
            }

            threadPartitionMergeAndSort(current_thread_id, secseglength, array2d); //completes phase 4,5

            break;
        }
    }
}

void *sortInLastThread(void *argc) // last thread will get size / THREAD_NUM + the remaining elements first
{

    int current_thread_id = threadIDAssign();

    int *array_head = (int *)argc;
    int seclength = size / THREAD_NUM + size % THREAD_NUM;

    int *temp = malloc(sizeof(int) * (seclength)); // temporary array to store the input elements
    for (int k = 0; k < seclength; k++)
    {
        temp[k] = *array_head;
        array_head++;
    }

    qsort(temp, seclength, sizeof(unsigned int), compare); // phase 1 sort

    int *sample = malloc(sizeof(int) * THREAD_NUM);

    for (int i = 0; i < THREAD_NUM; i++) // create sample array in phase 1
    {
        sample[i] = temp[i * size / (THREAD_NUM * THREAD_NUM)];
    }

    pthread_mutex_lock(&mutex);
    mergeAndSortSampleandSelectPivot(sample); // completes pahse 2
    pthread_mutex_unlock(&mutex);

    while (1)
    {
        if (callcount == THREAD_NUM * THREAD_NUM + 1) // continue only after phase 2 is done
        {

            int *subseq = malloc(sizeof(int) * (THREAD_NUM));
            subseq[0] = 0;
            int subseqidx = 1;

            for (int i = 0; i < THREAD_NUM; i++) // set subseqeunces legnth by the pivot
            {
                for (int j = 0; j < seclength; j++)
                {
                    if (temp[j] > pivot[i])
                    {
                        subseq[subseqidx] = j;
                        subseqidx++;
                        j = j;
                        break;
                    }
                }
            }

            int **array2d = malloc(sizeof(int *) * THREAD_NUM);
            int *secseglength = malloc(sizeof(int *) * THREAD_NUM);

            // define partition lengths
            for (int i = 0; i < THREAD_NUM - 1; i++)
            {
                secseglength[i] = subseq[i + 1] - subseq[i];
            }
            secseglength[THREAD_NUM - 1] = seclength - subseq[THREAD_NUM - 1];

            // allocate 2d array length accrodingly
            for (int i = 0; i < THREAD_NUM; i++)
            {
                array2d[i] = malloc(sizeof(int *) * secseglength[i]);
            }

            int count = 0;
            for (int i = 0; i < THREAD_NUM; i++)
            {
                for (int j = 0; j < secseglength[i]; j++) // completes phase 3
                {
                    array2d[i][j] = temp[count];
                    count++;
                }
            }

            threadPartitionMergeAndSort(current_thread_id, secseglength, array2d); // completes phase 4,5

            break;
        }
    }
}

int main(int argc, char **argv)
{
    long i, j;
    struct timeval start, end;

    if ((argc != 2 && argc != 3))
    {
        printf("Usage: ./psort <number> [<no_of_workers>]\n");
        exit(0);
    }

    size = atol(argv[1]);

    if (argc == 3)
    {
        thread_num = atol(argv[2]);
        if (thread_num != THREAD_NUM)
        {
            printf("Input worker thread is different from the system! current worker threads: %d\n", THREAD_NUM);
            printf("In order to change the number of worker threads, please adjust the global variable 'THREAD_NUM': line 23 and run it again. Thank you!\n");
            exit(0);
        }
    }

    intarr = (unsigned int *)malloc(size * sizeof(unsigned int));
    if (intarr == NULL)
    {
        perror("malloc");
        exit(0);
    }

    // set the random seed for generating a fixed random
    // sequence across different runs
    char *env = getenv("RANNUM"); // get the env variable
    if (!env)                     // if not exists
        srandom(3230);
    else
        srandom(atol(env));

    for (i = 0; i < size; i++)
    {
        intarr[i] = random();
    }

    // measure the start time
    gettimeofday(&start, NULL);

    // psort starts

    pthread_t th[THREAD_NUM];

    shared2darray = malloc(sizeof(int *) * THREAD_NUM);

    for (i = 0; i < THREAD_NUM; i++)
    {
        if (i == THREAD_NUM - 1)
        {
            pthread_create(th + i, NULL, &sortInLastThread, (void *)(intarr + i * (size / THREAD_NUM)));
        }
        else
        {
            pthread_create(th + i, NULL, &sortInThreads, (void *)(intarr + i * (size / THREAD_NUM)));
        }
    }

    for (i = 0; i < THREAD_NUM; i++)
    {
        pthread_join(th[i], NULL);
    }

    // measure the end time
    gettimeofday(&end, NULL);

    if (!checking(intarr, size))
    {
        printf("The array is not in sorted order!!\n");
    }

    printf("Total elapsed time: %.4f s\n", (end.tv_sec - start.tv_sec) * 1.0 + (end.tv_usec - start.tv_usec) / 1000000.0);

    free(intarr);
    return 0;
}

int compare(const void *a, const void *b)
{
    return (*(unsigned int *)a > *(unsigned int *)b) ? 1 : ((*(unsigned int *)a == *(unsigned int *)b) ? 0 : -1);
}

int checking(unsigned int *list, long size)
{
    long i;
    printf("First : %d\n", list[0]);
    printf("At 25%%: %d\n", list[size / 4]);
    printf("At 50%%: %d\n", list[size / 2]);
    printf("At 75%%: %d\n", list[3 * size / 4]);
    printf("Last  : %d\n", list[size - 1]);
    for (i = 0; i < size - 1; i++)
    {
        if (list[i] > list[i + 1])
        {
            return 0;
        }
    }
    return 1;
}

```
## Performance

|      | 1000000 | 5000000 | 10000000 | 50000000 | 100000000 |
| ---- | ------- | ------- | -------- | -------- | --------- |
| 1    | 0.4958s | 3.2580s | 6.0497s  | 32.5420s | 73.1311s  |
| 2    | 0.2635s | 1.5703s | 3.0676s  | 17.3437s | 37.2646s  |
| 3    | 0.2041s | 1.0743s | 2.1950s  | 11.6489s | 25.0934s  |
| 5    | 0.1660s | 0.6860s | 1.3968s  | 7.5195s  | 15.3621s  |
| 9    | 0.0910s | 0.4817s | 0.9768s  | 5.1251s  | 11.0081s  |
| 13   | 0.0866s | 0.3961s | 0.8415s  | 4.4817s  | 8.7737s   |
| 16   | 0.0826s | 0.3843s | 0.8247s  | 4.1879s  | 8.2316s   |

![performance]({{site.url}}/images/2022-11-10-SampleSortWithMutiThreading/performance.png)