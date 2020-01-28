# Session 1

creationDate: Mon, Jan_27

Creator: JUNJIE 

Title: Class_preview

---

- so we will follow the conceptual framework most popular with economists, the potential outcomes model, also known as **the (Neyman-)Rubin causal model. **
  - This framework underlies thousands of observational studies and experiments in economics, political science, and sociology.
  - the main idea of the potential outcomes model is that wach individuals has `2 potential outcomes` 



## DWB ADs problem

---

​				${y^0_i}$ = donation amount in the **control state** (no ad),

​				${y^1_1}$= donation amount in the **treatment state** (saw ad).

​				$y_i = y^1_i d_i + y^0_1(1-d_i) = y^{d_i}_i$ 

to estimate the causal effects $\rightarrow$ the basic units of the causality are the individual-level treament effect $y^1_i - y^0_i$ $\rightarrow$ which represents the difference in donation amount caused by exposure to ad. 

![image-20200127153906671](/Users/leijunjie/Library/Application Support/typora-user-images/image-20200127153906671.png)

 This is the study of ***identification***, what we can learn from an arbitarily large quantity of data. If we imagine our data as realizations of random variables, then the average of any variable in the sample will be close to the *expectation* of the random variable. We know this because of the ***law of large numbers***. Now we can work in terms of the random variables, which will make our analysis tractable and lets us focus on the essential issues in causality and experimentation

## Building probability model for potential outcomes

---

#### (ATE --> average treatment effect)            

  $$E[\Delta] = E[Y^1 - Y^0]$$

$$Y_i = Y_i^1 D_i + Y_1^0(1 - D_i)$$ 

variable D $\rightarrow$ the treatment state

variable $Y^1 \& Y^0 $ are the representation of $y^0_i  \quad\&  \quad  y^1_i$  

## variation of average treatment effects

---

![image-20200127160841971](/Users/leijunjie/Library/Application Support/typora-user-images/image-20200127160841971.png)

$D = 1$ received the treatment

$D= 0$ did not receive the treatment

## Bias of the naive average treatment effect

---

NATE $\rightarrow$ Naive Average Treatment Effect; 

- ***seleciton bias*** 
- ***differential effect bias***

![image-20200127161240701](/Users/leijunjie/Library/Application Support/typora-user-images/image-20200127161240701.png)

### selection bias

systematic difference between  the ***treatment*** and ***control*** groups; i.e: people intentioally search for the DWB

### differential effect bias

the difference between how the ***treatment*** affects the ***treated group*** and how the treatment would affect the ***control group.*** i.e: this type of bias is typical in cases where some form of the agency is involved; 

- opt into the training program $\rightarrow$ the units ***opt*** into the treatmnet based on expectations about its individual effects;  
- expecation of the indiciual effects $\rightarrow$ the treatment are ***assigned*** to units based on the expectations about the individual effects; i.e: the market agency of DWB knows the person who searched for DWB and sepcifically & target them on this!



## Randomized experiments remove bias by enforcing independence

---









## Conditioanl Independece

---

CIA $\rightarrow$ ***conditional independence assumption*** alsocalled `selection on obsevables (especially in econometrics)` we can account for selection bias using observables 







