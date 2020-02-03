# ECON 626 Causal Inference

## Problem Set 1

Creator: Junjie Lei

Creation Date: Jan_28_2020

Title: Intro Causality Fundamentals

---

## 1 Notebook problems

### 1.1 Law of large numbers

1. Where in the code are the parameters μ and $\sigma^2$ set? 

   - the `data generating sequence` is set via the `np.random.seed(94115)` $\rightarrow $ DGP 

   - The $\mu$ & $\sigma^2$ is set via the line of  code 

     ```python
     X_n = np.random.normal(loc=0, scale=1, size=(10 ** 3, 1))
     # `numpy.random.normal`draws random sample from a normal(Gaussian) distribution; 
     # loc indicates the --> mean
     # scale --> standard deviation 
     ```

2. In the code, what objects correspond to

   -  $X_1$, $X_2$  ...

   - $S_n$,

   - $\dfrac{1}{n}S_n$? 

   -  Why are these objects in math not exactly the same as in a computer?

     

   1. the code responding to $X_1$, $X_2$  .. is 

      ```python
      X_n = np.random.normal(loc=0, scale=1, size=(10 ** 3, 1))
      ```

   2. the code respoding to $$S_n = \sum_{i=1}^nX_i$$ is 

      ```python
      partial_sums = X_n.cumsum(axis= 0)
      ```

   3. the code responding to $\dfrac{1}{n}S_n$ is 

      ```python
      partial_means = np.divide(partial_sums, n_index)
      ```

   4. the object in math in not the *exactly* the same in a computer; 

      - `python` index starts from `0` instead of `1`

      -  It is a `parital mean` and `parital sum`, but mathematical calculation does not require those; 

        

3. Explain intuitively why the line is more stable at the end.

   becasue of the ***Law of large numbers***;  if we calculate the average of a large data set it should be very close to the theoretical mean of the underlying distribution.

### 1.2 Selection bias

<img src="/Users/leijunjie/Library/Application Support/typora-user-images/image-20200131120926317.png" alt="image-20200131120926317" style="zoom:50%;" />

1. calculate the $$E[Y^1 - Y^0]$$

   - **ATE** $\rightarrow$ the average treatment effect aberaged across the entire population. 

   - should be `0`

2. calculate the **NATE**

   $NATE \rightarrow E[Y^1|D=1] - E[Y^0|D=0] $

   hence it is $0.814968 - 0.642790$ $\rightarrow$ `0.172178` 

3. calculate the **Selection bias** 

   - **Selection bias** $\rightarrow$ $E[Y^0|treatment grp] - E[Y^0|control grp]$

     hence it is $0.814968 - 0.642790$ $\rightarrow$ `0.172178` 

4. what line(s) of code **fundamentally ** craetes the selection bias? explain.

   ```python
   charitability = np.random.normal(loc=-1, scale=0.5, size=N)
   ```

   and the following lines of code determines if the individuals search for the DWB ads and receive the `treatment`;

   ```python
   prob_search_dwb = scipy.special.expit(charitability)
   ```

   `selction bias` is the systematic difference in $Y^0$ between the treatment and control groups. It occurs when the two groups are systematically different even in hte absence of an actual treatment, in this case, it is the `charitibility` of different indiviudals;

   

5. calculate the **$P(D =1 )$**

   ![Screen Shot 2020-01-28 at 8.53.36 PM](/Users/leijunjie/Desktop/Screen Shot 2020-01-28 at 8.53.36 PM.png)

### 1.3 Data generating process for a null experiment

1. Make a version of this data-generating process that is a true randomized experiment. The new treatment variable should be $D_{exp}$. Make the overall probability of treatment the same, i.e., $P(D =1) = P(D_{exp} = 1)$. Show that the experiment eliminates **selection** **bias**. Calculate the ***NATE***.

```python
# size of the experiment;  
N = 10 ** 6 

# inherient charity;
charitability = np.random.normal(loc = -1, scale=0.5, size = N)

# AD in this case has no effect;
Y0 = np.random.lognormal(mean=(charitability + 0.5), sigma=0.1, size=N)
Y1 = Y0.copy()

# the treatment is the AD
# in this exercise, we are going to make the AD amount the same; 
# prob_search_dwb = scipy.special.expit(charitability)
D = np.random.binomial(n = 1, p = 0.5, size = N)

np.unique(D, 
          return_counts = True)
```

<img src="/Users/leijunjie/Library/Application Support/typora-user-images/image-20200131112734625.png" alt="image-20200131112734625" style="zoom:40%;" />

- $NATE \rightarrow E[Y^1 | D = 1 ] - E[Y^0 | D = 0] $

  $= - 0.000705$

2. Interpret figure 1 'density of $Y_0$ by treatment group' w.r.t the independece assumption: 

   ($Y^1$, $Y^0$ $\bot$ $D$ )

   even when `selection bias` is eliminated $\rightarrow $ (the potential outcomes of donation without seeing the ads are jointly indepedent of the treatment assignment.)

   the treatment & control groups bahave differently even if there is no ads

3. 

<img src="/Users/leijunjie/Library/Application Support/typora-user-images/image-20200131121506802.png" alt="image-20200131121506802" style="zoom:25%;" /><img src="/Users/leijunjie/Library/Application Support/typora-user-images/image-20200131121408184.png" alt="image-20200131121408184" style="zoom:25%;" /> 

the overlap...



### 1.4 Data-generating process for an experiment with a treatment effect

1. make a version of the biased data-generating process with an $ATE = 0.3$

   the line of the code that I changed is 

   ```python
   Y1 = Y0 + 0.3
   NATE --> 
   1.11414 - 0.64309 = 0.47104999999999986 
   ```

2. Repeat the Dexp experiment to show that NATE=ATE=0.3.

   <img src="/Users/leijunjie/Library/Application Support/typora-user-images/image-20200201153123311.png" alt="image-20200201153123311" style="zoom:80%;" />



## 2 Causality warm-up

### 1. 

1. Selection bias;

   - Control for all other factors, `selection bias` exisits when there are systematic differences in 2 groups. i.e. certain charateristics crucial to the output of the experiment exists in one group but not  in another; even without any treament, 2 groups of individuals make very different decisions;

   - mathmatically

     NATE = ATE + selection bias + differential bias; 

     $E[Y^0|treatment] - E[Y^0|control] $

   -  it is a problem, because of the bias exists in the system, hence the true treatment effect cannot be observed or misguided; 

2. 
   - It could be `cultural differences`; people with different background, religious, origins hold very different opinions on various situations or when facing the experiment; 
   - it could also exists in the `sample selection process`; also selection bias can occur if the experimenters have some biases when selecting the data; 



3. random variable to be independent; 
   - one variable's change ($ \Delta$ )does not rely/depend on all other variables' movements; 
   - mathematically; it uses the symbol $(\bot) $



## 3 In-class study discussion

## 4 Critical reading

## 5 Selection bias vs. big data

1. write down the $NATE$ decomposition equation and write the value of each of the 4 components

   $$\textbf{NATE} = \textbf{ATE} (E[Y^1 - Y^0] ) + \textbf{SelectionBias} + \textbf{Differnential Bias}$$

   * NATE --> $E[Y1|D=1] - E[Y0|D=0] = 0.17217832824951973$
   * ATE --> $E[Y^1 - Y^0] = 0$  
   * S.B --> $ 0.17217832824951973$
   * D.B --> $0$ 

2. 

## Bonus.1 Why it is called identification

## Bonus.2 



---

Junjie