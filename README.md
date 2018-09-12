# MDI343 Challenge: Sparse Score Fusion for Classifying Mate Pairs of Images

### Introduction

####  Challenge MDI343 2017-2018
##### Authors :  Umut Şimşekli & Stéphane Gentric

The topic of this challenge will be determining if two images belong to the same person or not. Conventionally, in order to solve this task, one typically builds an algorithm to provide a "score" for a given image pair. If the value of the score is high, it means that it is more probable that these images belong to the same person. Then, by using this score, one can determine if the images belong to the same person, by simply thresholding it.   

The goal of this challenge is to build a system for determining if two images belong to the same person or not by "fusing" multiple algorithms. In particular, for a given image pair, you will be provided the scores obtained from **14** different algorithms, each of which has a different computational complexity.   

Then the aim is to combine the scores of these algorithms (in a way that will be described in the sequel) in order to obtain a better classification accuracy. However, there will be a strict **computational budget**, such the running times of the algorithms that you combine **cannot exceed a certain time threshold**. For example, let $t_i$ denote the running time of algorithm $i$ in milliseconds ($i = 1,\dots,14$). Then, you will be given a threshold, $T$, such that the total computational time of the algorithms that you combine will not exceed $T$: 

$
\sum_{i\in C} t_i \leq T,
$

where $C \subset \{1,\dots,14\}$ is the set of algorithms that you choose to combine. The idea in such fusion is that "combining several fast algorithms might be better than using a single slow (possible more complex) algorithm". 

Before we describe how the fusion will be done, let us introduce the data:

**Training data:**

There will be $N= 2048853$ image pairs in the dataset. For a given image pair $n \in \{1,\dots,N\}$, we define $y_n = 1$ if this image pair belongs to the same person, or $y_n=0$ otherwise.

We then define a vector of scores for each image pair, $s_n \in \mathbb{R}_+^{14}$, such that $i$th component of $s_n$ will encode the score obtained by the $i$th algorithm, for the given image pair.  

**Test data:**

The test data contain $N_\text{test} = 170738$ image pairs. Similarly to the training data, each image pair contains a label and a vector of scores that are obtained from $14$ different algorithms. The test data will not be provided.

### Fusion Method 

In this challenge, you are expected to build a fusion system that is given as follows. Given a score vector $s \in \mathbb{R}_+^{14}$, we first define an extended vector $s'$, by appending a $1$ in the beginning of the original vector $s\in \mathbb{R}_+^{15}$: $s' = [1, s]$. Then we use the following fusion scheme in order to obtain the combined score $\hat{s}$: 

$
\hat{s} = s'^\top M s' 
$

where $M \in \mathbb{R}^{15 \times 15}$, is the "fusion matrix". This matrix will enable you to combine the scores of the different algorithms in a linear or a quadratic way. 

### The goal and the performance criterion

In this challenge, we will use an evaluation metric, which is commonly used in biometrics, namely the False Recognition Rate (FRR) at a fixed False Acceptance Rate (FAR). **The lower the better.**

The definitions of these quantities are as follows: (definitions from Webopedia)

**The false acceptance rate**, or **FAR**, is the measure of the likelihood that the biometric security system will incorrectly accept an access attempt by an unauthorized user. A system’s FAR typically is stated as the ratio of the number of false acceptances divided by the number of identification attempts.

**The false recognition rate**, or **FRR**, is the measure of the likelihood that the biometric security system will incorrectly reject an access attempt by an authorized user. A system’s FRR typically is stated as the ratio of the number of false recognitions divided by the number of identification attempts.

In this challenge, we will use the following evaluation scheme:

1) Given the scores, find a threshold that will give an FAR 0.01 %

2) Given the threshold, compute the FRR

The overall metric will be called **"the FRR at 0.01% FAR"**.