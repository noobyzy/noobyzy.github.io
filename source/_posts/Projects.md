---
title: Projects
date: 2024-08-25 16:58:04
tags:
    - 记录
---

In this post, I will record the projects I have done.

# 2024

## **Teacher's Assistant in CS4017 Introduction to Optimization**

### Keywords
`Convex Optimization`, `CVXPY`


### Project Description
I was honored to be the TA of Introduction to Optimization. During the semester, I was responsible for holding discussion lectures, office hours, and grading the homeworks and exams. I helped students understand the basic concepts in convex optimization and manipulation of CVXPY.

### Project Link
Preliminaries: calculus and linear algebra
{% pdf ./CS4017_Lecture_0_Review_of_Calculus_and_Linear_Algebra.pdf %}


Introduction to CVXPY
{% pdf ./CS4017_Intro_to_CVXPY.pdf %}

---
---

# 2023

## **Communication-Efficient Distributed Optimization with Adaptability to System Heterogeneity**

### Keywords
`Distributed Optimization`, `ADMM`, `Heterogeneity`, `Python`


### Project Description
We consider the setting of agents cooperatively minimizing the sum of local objectives plus a regularizer on a graph. This paper proposes a primal-dual method in consideration of three distinctive attributes of real-life multi-agent systems, namely: (i) expensive communication, (ii) lack of synchronization, and (iii) system heterogeneity. In specific, we propose a distributed asynchronous algorithm with minimal communication cost, in which users commit variable amounts of local work on their respective subproblems. We illustrate this both theoretically and experimentally in the machine learning setting, where the agents hold private data and use a stochastic Newton method as the local solver. Under standard assumptions on Lipschitz continuous gradients and strong convexity, our analysis establishes linear convergence in expectation and characterizes the dependency of the rate on the number of local iterations. We proceed a step further to propose a simple means for tuning agents' hyperparameters locally, so as to adjust to heterogeneity and accelerate the overall convergence. Last, we validate our proposed method on a benchmark machine learning dataset to illustrate the merits in terms of computation, communication, and run-time saving as well as adaptability to heterogeneity.

This work was accepted by IEEE Conference on Decision and Control (CDC), 2023.

### Project Link
[arXiv](https://arxiv.org/html/2308.05395v3)

[IEEE CDC](https://ieeexplore.ieee.org/document/10383755)


### Project Result
![](DRUIDVL.png)


## **A python (numpy) implementation of linear / logistic regression models, optimized with Gradient Descent / Newton's method.**

### Keywords
`Linear Regression`, `Logistic Regression`, `Gradient Descent`, `Newton's method`, `Convex Optimization`, `Python`


### Project Description
A python (numpy) implementation of linear / logistic regression models, optimized with Gradient Descent / Newton's method. An comparison with optimal solution (solved by CVXPY) is also provided.

### Project Link
[Optimization-Theory](https://github.com/noobyzy/Optimization-Theory)


### Project Result
![](linreg_weights.png)

---
---

# 2022

## **Communication-Efficient Stochastic Zeroth-Order Optimization for Federated Learning**

### Keywords
`Federated Learning`, `Zeroth-order Optimization`, `AirComp`, `Python`


### Project Description
Federated learning (FL), as an emerging edge artificial intelligence paradigm, enables many edge devices to collaboratively train a global model without sharing their private data. To enhance the training efficiency of FL, various algorithms have been proposed, ranging from first-order to second-order methods. However, these algorithms cannot be applied in scenarios where the gradient information is not available, e.g., federated black-box attack and federated hyperparameter tuning. To address this issue, in this paper we propose a derivative-free federated zeroth-order optimization (FedZO) algorithm featured by performing multiple local updates based on stochastic gradient estimators in each communication round and enabling partial device participation. Under non-convex settings, we derive the convergence performance of the FedZO algorithm on non-independent and identically distributed data and characterize the impact of the numbers of local iterates and participating edge devices on the convergence. To enable communication-efficient FedZO over wireless networks, we further propose an over-the-air computation (AirComp) assisted FedZO algorithm. With an appropriate transceiver design, we show that the convergence of AirComp-assisted FedZO can still be preserved under certain signal-to-noise ratio conditions. Simulation results demonstrate the effectiveness of the FedZO algorithm and validate the theoretical observations.

This work was accepted by IEEE Transactions on Signal Processing (TSP), 2022.

### Project Link
[arXiv](https://arxiv.org/abs/2201.09531)

[IEEE TSP](https://ieeexplore.ieee.org/document/9917343)

[code](https://github.com/noobyzy/FedZO)


### Project Result
![](FedZO.png)


---
---

# 2021

## **Teacher's Assistant in CS110 Computer Architecture**

### Keywords
`Computer Architecture`, `RISC-V`, `C`, `C++`


### Project Description
I was honored to be part of the TA group of CS110. During the semester, I was responsible for holding discussion lectures, lab sessions, and office hours. I was also in charge of designing and grading (some of) the homeworks and exams. I helped students understand the basic concepts of computer architecture, such as the RISC-V instruction set, pipelining, and cache. I also helped students with their programming assignments, which were written in C and C++. See the course website for more information: [CS110](https://robotics.shanghaitech.edu.cn/courses/ca/21s/).

### Project Link
[HW5: CACoin Mining with POSIX Threads](https://robotics.shanghaitech.edu.cn/courses/ca/21s/homeworks/5/): This homework requires students to accelerate the mining process of CACoin by optimizing and parallelizing the code with *pthread*. 

[Project 4: Longan Nano Retro Snake Game](https://robotics.shanghaitech.edu.cn/courses/ca/21s/projects/4/): This project requires students to implement a retro snake game on the Longan Nano board, using *C* and *RISC-V* assembly language. The illustrated game was made by me.

## **Reproduction of Upper bounds for Model-Free Row-Sparse Principal Component Analysis.**

### Keywords
`Machine Learning`, `Row-Sparse PCA`, `Integer Programming`, `Python`


### Project Description
This report focus on the reproduction of [**Upper bounds for Model-Free Row-Sparse Principal Component Analysis**](https://proceedings.mlr.press/v119/wang20e.html).This work proposed a new framework that finds upper (dual) bounds for the sparse PCA within polynomial time via solving a convex integer program (IP). The convex IP is guaranteed to give a dual bound within an affine function of the global optimal value. A monotone local search algorithm is also proposed here to give a good feasible solution for SPCA, which works as the lower (primal) bound of the origin problem. In general, this work presents a complete solution procedure of generating good solutions and proving quality of these solutions. There is currently no comparable theoretical or computational results for solving model-free SPCA comparing to this work.

### Project Link
[RowSPCA-CS282](https://github.com/noobyzy/RowSPCA-CS282)


### Project Result
![](rPCA.png)


## **Implementation of Primal Simplex Method**

### Keywords
`Linear Programming`, `Classical Simplex`, `Revised Simplex`, `Python`


### Project Description
Simplex method has been a powerful weapon against linear programming problem ever since it was raised. The idea of simplex is to jump from one feasible solution to another, continuously decreasing the objective value until it reaches a global optima, which gives us the optimal feasible solution. Many genres of simplex method have been inspired, including primal classical method, primal revised method and dual simplex method. In this project, I tried to implement primal classical method and primal revised method. Besides, to better start the simplex method, I implemented the two-stage method. A ’Bland’ method is also added here to avoid degenerancy.

### Project Link
[Primal_Simplex_Solver](https://github.com/noobyzy/Primal_Simplex_Solver)

### Project Result
![](simplex.png)


## **Pinyin to Hanzi: A Chinese Input Method Based on Statistical Learning**

### Keywords
`NLP`, `Input Method`, `Hidden Markov Model`, `Python`


### Project Description
Recent years have witnessed sensational progress of Chinese input method driven by some mature techniques, such as dictionary matching, statistical learning and neural networks. The core task of it is translating the raw pinyin series into Chinese characters with valid and reasonable grammar. In this work, we dedicate to develop a demo of Chinese input method by mainly utilizing Hidden Markov Model through three stages: segmentation, tokenization and translation. We verify the empirical performance of our model on test data and explore some insights from the results.

### Project Link
[PY2HZ](https://github.com/noobyzy/PY2HZ)


### Project Result
| Number of tokens | Accuracy Type |   Accuracy (\%)   |                   |                   |
|:---------------------------:|:------------------------:|:-----------------:|:-----------------:|:-----------------:|
|                             |                          | Top1 | Top3 | Top5 |
|             3-5             |             W            |       0.782       |       0.826       |       0.837       |
|                             |             S            |       0.310       |       0.375       |       0.395       |
|             6-8             |             W            |       0.827       |       0.855       |       0.863       |
|                             |             S            |       0.230       |       0.285       |       0.295       |
|            >=9           |             W            |       0.824       |       0.854       |       0.864       |
|                             |             S            |       0.085       |       0.115       |       0.135       |

---
---

# 2020

## **Tetrahedralization and Volume Rendering**

### Keywords
`Computer Graphics`, `Rendering`, `C++`


### Project Description
Volume visualization is useful in many areas. We focus on directly rendering unstructured tetrahedral meshed where volume’s interior needs to be visualized. There are many approaches to rendering unstructured grids and accuracy is usually important. Ray-casting based methods, which our work focuses on, are widely accepted. Tetrahedralization means to collect data into multiple tetrahedrons, while some datasets have already implemented this process. Tetrahedralized volume rendering has a better usage of input data and a more efficient calculation especially in the transparent area. Whereas its intuition and procedure are very much familiar to the former one’s.

### Project Link
[Tetrahedron-Volume-Rendering](https://github.com/noobyzy/Tetrahedron-Volume-Rendering)


### Project Result
![](render_result.png)

