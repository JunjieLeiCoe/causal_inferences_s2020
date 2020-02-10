# Theory

1. Identification; 

   - with infinite data 

     

2. Statitics 

   - limited data obs 

     

3. Covariance



---



# Practise 

- identiifcation



- statistics





# In Class Notes



- assum: correctly randomized the NATE =ATE 

  `size = (rows, cols)`

  `10 ** 3, 1`

  

- `reshape(-1, 1)` $\rightarrow$ vector to array; 

  

- pd.DataFrame(`array`).plot(legend = False)





- 

  ```python
  # Let's package the simulation in a function.
  
  def simulate_lln(distribution, sequence_length=10 ** 3):
    # Generate the sequence {X_i}
    X_n = distribution(size=(sequence_length, 1))
    # Calculate the partial sums S_n = sum of X_i from i=1 to i=n
    partial_sums = X_n.cumsum(axis=0)
    # Calculate the partial sums for all n
    n_index = np.arange(start=1, stop=len(partial_sums)+1).reshape((-1, 1))
    # Divide by {n} to get the partial averages
    partial_means = np.divide(partial_sums, n_index)
  
    means = pd.DataFrame(partial_means, index=n_index[:,0], columns=['mean_n'])
    return means
  ```

  distribution

  

- `np.random.binomial(n = 1, p = probability)`

  flip one coin --> 0 or 1 for n = 1



---

新课部分



-  why simulation

- random sampling
  - 1 simple random sample --> whole; 
  - data point/obs point

- repeated sampling
  - replication of the sample; 

- propety of **Central Limit Theorem**



- random var distribution expression; 

  



- 

- 

  

  

  

  

  

  

  

  

  

  

  

   

  



























