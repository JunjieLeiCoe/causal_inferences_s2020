# ECON 626 Causal Inference

## Problem Set 1

Creator: Junjie Lei

Creation Date: Jan_28_2020

Title: Intro Causality Fundamentals

---

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
      partial_sums = X_n.cumsum(axis=0)
      ```

   3. the code responding to $\dfrac{1}{n}S_n$ is 

      ```python
      partial_means = np.divide(partial_sums, n_index)
      ```

   4. the object in math in not the exactly the same

3. 