<h1 style="text-align: center;" markdown="3">Causal Inference Assignment 2</h1>
Creator: <u>Junjie Lei</u>

CreationDate: <u>Feb_7/2020</u>

Title: <u>*Random Sampling*</u>

---

<h2 style="text-align: center;" markdown="5">1. Notebook from class</h2>
<h3 style="text-align: left;" markdown="2">1.1 Simulating basic distributions   class</h3>
1. 

```python
n = 10000
x_i = np.random.binomial(n=1, p = 0.25, size = n )

print("mean for the bionomial = ", x_i.mean())
print('p = 0.25')
print("std for the binomial = ", x_i.std())
```

output:

<img src="C:\Users\28260\AppData\Roaming\Typora\typora-user-images\image-20200210103111642.png" alt="image-20200210103111642" style="zoom:100%;" />



```python
n = 10000
x_i = np.random.exponential(scale=3.0, size= n )

print("mean for the exp function = ", x_i.mean())
print('p = 0.25')
print("std for the binomial = ", x_i.std())
```



output: 

<img src="C:\Users\28260\AppData\Roaming\Typora\typora-user-images\image-20200210103210357.png" alt="image-20200210103210357" style="zoom:100%;" />

<img src="C:\Users\28260\AppData\Roaming\Typora\typora-user-images\image-20200210103322529.png" alt="image-20200210103322529" style="zoom:50%;" />

<img src="C:\Users\28260\AppData\Roaming\Typora\typora-user-images\image-20200210103339690.png" alt="image-20200210103339690" style="zoom:50%;" />



2. plot the underlying mean on the graph

   for binomial plot

```python
ax = sample_means_bi.hist(bins=100, density=True)


ax.axvline(mu, color='black', linestyle='--')
ax.text(x=mu, y=7, s=r'$\mu$', fontsize=16)
ax.set_xlabel(r'$\bar{X}$', fontsize=16)
ax.set_ylabel('Density', fontsize=16)
ax.set_title("Simulating the sampling distribution of the mean", fontsize=16)
ax.legend(fontsize=22)
ax.tick_params(labelsize=16)
```



<img src="C:\Users\28260\AppData\Roaming\Typora\typora-user-images\image-20200210153123288.png" alt="image-20200210153123288" style="zoom:50%;" />

​	for exponential 

```python
ax = sample_means_exp.hist(bins=100, density=True)
'''and other formatting code'''
```

<img src="C:\Users\28260\AppData\Roaming\Typora\typora-user-images\image-20200210153241543.png" alt="image-20200210153241543" style="zoom:50%;" />





<h3 style="text-align: left;" markdown="2">1.2 Simulating experiment statistics</h3>



1. the code `sample_results.describe()` showed the standard deviation is  $ 0.127811 $

2.  

   - change the simulation so that the split is always $20 \% \& 80\%$ 

     parameter `share_treatment` indicates the the proportion of N in the treatment group

     ```python
     share_treatment = 0.2
     ```

     

     the plot of the $20 \%$ treatment and $80 \%$  control

   <img src="C:\Users\28260\AppData\Roaming\Typora\typora-user-images\image-20200210105423644.png" alt="image-20200210105423644" style="zoom:80%;" />

   ​		

   ​		the standard deviation is   $0.158808 $

   

   - the noise `standard deviation` becomes smaller when estimating the `ATE` if we use the $20 \% \& 80 \% $  to split the  treatment and control group. 

   

   <img src="C:\Users\28260\AppData\Roaming\Typora\typora-user-images\image-20200210154353318.png" alt="image-20200210154353318" style="zoom:55%;" /> <img src="C:\Users\28260\AppData\Roaming\Typora\typora-user-images\image-20200210154414622.png" alt="image-20200210154414622" style="zoom:55%;" />

   - the best way to split is to do the $50/50$, because the std.ev is the minimum when the shared treatment is split into $50/50$

3. the estimated `ATE` is greater than $0.7$ 

   I used the following to calculate the 

   ```python
   count_ate = sample_results.groupby(['diff']).apply(lambda x : (x >= 0.7))
   count_ate.groupby(['diff']).count().reset_index()
   ```

   the percentage is `6.06%`

   <img src="C:\Users\28260\AppData\Roaming\Typora\typora-user-images\image-20200210113434159.png" alt="image-20200210113434159" style="zoom:80%;" />



4. 2 standard deviation would be $2 \times  0.128988   =  0.257976 $ away

   hence we can calculate the percentage $\geq 2 std$

   ```python
   count_ate_1 = sample_results.groupby(['diff']).apply(lambda x : (x >= 0.757976 ))
   count_ate_1.groupby(['diff']).count().reset_index()
   ```

   <img src="C:\Users\28260\AppData\Roaming\Typora\typora-user-images\image-20200210114350850.png" alt="image-20200210114350850" style="zoom:80%;" />

   if larger than 2 $std.ev()$ it would be  `2.18%`

​		if lower than 2 $std.ev()$ it would be `2.61% `

​		<img src="C:\Users\28260\AppData\Roaming\Typora\typora-user-images\image-20200210114741046.png" alt="image-20200210114741046" style="zoom:80%;" />








<h2 style="text-align: center;" markdown="5">2. Simulating t-tests under the null</h2>

1. the probability that when given the null hypothesis is true and we still reject the null;

   when it is refers to the process of generating sample data, it is the probability that this process rejects $H_{0}$ when the underlying $H_{0}$ simulated is true.



2. 

```python
# Simulation results over a grid of parameter values
B = 5000
ATE = 0.0
parameter_grid = [
    dict(B = B, n = 400, expectation_Y0 = 1, ATE = ATE, share_treatment = 0.10, sigma = 1, alpha = 0.05),
    dict(B = B, n = 400, expectation_Y0 = 1, ATE = ATE, share_treatment = 0.25, sigma = 1, alpha = 0.05),
    dict(B = B, n = 400, expectation_Y0 = 1, ATE = ATE, share_treatment = 0.50, sigma = 1, alpha = 0.05),
    dict(B = B, n = 400, expectation_Y0 = 1, ATE = ATE, share_treatment = 0.75, sigma = 1, alpha = 0.05),
    dict(B = B, n = 400, expectation_Y0 = 1, ATE = ATE, share_treatment = 0.90, sigma = 1, alpha = 0.05),
]

simulation_results =  pd.DataFrame([get_simulation_results(**params) for params in parameter_grid])

# We can also test that the simulations worked as expected.
# For example:
assert((simulation_results['share_treatment'] == simulation_results.eval("N_treatment / N")).all())
```

---

sample output:

<img src="C:\Users\28260\AppData\Roaming\Typora\typora-user-images\image-20200210141914821.png" alt="image-20200210141914821" style="zoom:100%;" />

- diff becomes more scattered when the shared_treatment goes to 0.75 and 0.9

- rejection rate wave above and down the 0.05.

- the changes of t's distribution is not that significant, no pattern can be observed.



3. 

<img src="C:\Users\28260\AppData\Roaming\Typora\typora-user-images\image-20200210142855448.png" alt="image-20200210142855448" style="zoom:150%;" />





4. 

![image-20200210143233910](C:\Users\28260\AppData\Roaming\Typora\typora-user-images\image-20200210143233910.png)

- yes, the rejection rate changed



5. 

![image-20200210143752780](C:\Users\28260\AppData\Roaming\Typora\typora-user-images\image-20200210143752780.png)



6. 
   the false positive rate is the threshold analysts set to determine whether we should reject a null hypothesis or not. 

   in the simulation results we got above, we can see that the distribution changes while we change some of the parameters. so we need to determine upfront a threshold to evaluate the distribution or the test results.




<h2 
    style="text-align: center;" markdown="1">3. Simulating t-tests when there is an ATE
</h2>



1.  Power is the probability when we rejects a hypothesis test with effect size when, in fact, it is false. That is, Power is the probability of making a correct decision (to reject the null hypothesis) when the null hypothesis is false. It is often affected by sample size, effect size, and alpha.  But not necessarily under the control  of analyst.



2. 
   here is the code chunk that I modified; it takes really long time to simulate the process; 

   ```python
   B = 5000
   N = 1000
   ATE = 0.15
   expectation_Y0 = 3
   sigma = 1
   share_treatment = 0.5
   parameter_grid = [
       dict(B = B, n = N, expectation_Y0 = expectation_Y0, ATE = ATE, share_treatment = share_treatment, sigma = sigma, alpha = 0.01),
       dict(B = B, n = N, expectation_Y0 = expectation_Y0, ATE = ATE, share_treatment = share_treatment, sigma = sigma, alpha = 0.05),
       dict(B = B, n = N, expectation_Y0 = expectation_Y0, ATE = ATE, share_treatment = share_treatment, sigma = sigma, alpha = 0.10),
       dict(B = B, n = N, expectation_Y0 = expectation_Y0, ATE = ATE, share_treatment = share_treatment, sigma = sigma, alpha = 0.15),
       dict(B = B, n = N, expectation_Y0 = expectation_Y0, ATE = ATE, share_treatment = share_treatment, sigma = sigma, alpha = 0.25),
       dict(B = B, n = N, expectation_Y0 = expectation_Y0, ATE = ATE, share_treatment = share_treatment, sigma = sigma, alpha = 0.35),
   ]
   
   simulation_results =  pd.DataFrame([get_simulation_results(**params) for params in parameter_grid])
   ```

   ---

   sample Input/Output:

   ![image-20200210130337013](C:\Users\28260\AppData\Roaming\Typora\typora-user-images\image-20200210130337013.png)

   

   as the $\alpha$ goes up, we can see that the $P_{reject}$ parameter increase but at a decrease rate; 

   - simulated power is p_reject; that is the mean of probability of reject the null hypothesis
   - false positive rate is set as alpha

   

   here is the plot  between the relationship  between the alpha and power

   ***code and output*** 

   ```python
   plt.plot(simulation_results['alpha'], simulation_results['p_reject'])
   font2 = {'family' : 'Times New Roman',
   'weight' : 'normal',
   'size'   : 15,
   }
   plt.xlabel('alpha',font2)
   plt.ylabel('simulated_power',font2)
   
   plt.show()
   ```

   

   

   ![image-20200210130728883](C:\Users\28260\AppData\Roaming\Typora\typora-user-images\image-20200210130728883.png)

   

   

   3. in this task, I vary the treatment group percentage 

   ```python
   B = 5000
   N = 1000
   ATE = 0.15
   expectation_Y0 = 3
   sigma = 1
   alpha = 0.05
   parameter_grid = [
       dict(B = B, n = N, expectation_Y0 = expectation_Y0, ATE = ATE, share_treatment = 0.1, sigma = sigma, alpha = alpha),
       dict(B = B, n = N, expectation_Y0 = expectation_Y0, ATE = ATE, share_treatment = 0.25, sigma = sigma, alpha = alpha),
       dict(B = B, n = N, expectation_Y0 = expectation_Y0, ATE = ATE, share_treatment = 0.5, sigma = sigma, alpha = alpha),
       dict(B = B, n = N, expectation_Y0 = expectation_Y0, ATE = ATE, share_treatment = 0.75, sigma = sigma, alpha = alpha),
       dict(B = B, n = N, expectation_Y0 = expectation_Y0, ATE = ATE, share_treatment = 0.9, sigma = sigma, alpha = alpha),
   ]
   
   simulation_results =  pd.DataFrame([get_simulation_results(**params) for params in parameter_grid])
   ```

   

   ---

   and the code for plot & plot

   ```python
   plt.plot(simulation_results['share_treatment'], simulation_results['p_reject'])
   font2 = {'family' : 'Times New Roman',
   'weight' : 'normal',
   'size'   : 15,
   }
   plt.xlabel('share_treatment',font2)
   plt.ylabel('simulated_power',font2)
   
   plt.show()
   ```

   ![image-20200210131407722](C:\Users\28260\AppData\Roaming\Typora\typora-user-images\image-20200210131407722.png)

   

   

   

   4. 

      <u>***- Under Scenario 1***</u>

      Because the false positive rate is the probability that rejects the null hypothesis in favor of a false alternative hypothesis, that is the probability of a Type I error. The power is the probability that rejects the null hypothesis when, in fact, it is false. That is, the probability of denying the Type II error. Therefore Power equals to (1-probability(Type II error)). According to the question, a full launch have a very large up-front cost, hence, the product manager's desire outcome must be the most utility of this A/B Test. Therefore, we want both the probability of Type I error and of Type II error as small as possible. It means that power should be bigger and alpha should be smaller.

      - However, as we plot above, power gets smaller while alpha gets smaller. So we have to find the balance between this two statistics. We should notice that margin value when alpha changes from 0.01 to 0.05 is the max.

      - also, in statistical history, the default value of alpha is 0.05.

      In summary, alpha = 0.05 is our best choice.

      <u>***-  Under Scenario 2***</u>

      in this circumstance, the false positive rate I recommend is 0.01.

      As we discussed in (i), we want false positive rate to to the smallest and the power to be biggest.

      Because we can launch without any additional costs, that is, we can expand the size of population that participant in this A/B Test (in this case is N and B) to enlarge the value of power. Therefore, we do not have to sacrifice false positive rate to get higher power. We can choose the smallest value of the false positive rate to get the max utility.

   

   5. 

      1. 

      ```python
      simulated_power = simulation_results.query('alpha==0.05')['p_reject'][1]
      # Example of how to calculate the power for a given point.
      calculated_power = power.tt_ind_solve_power(
          effect_size=ATE / sigma,
          alpha=0.05,
          nobs1=N / 2,
          ratio=1.0,
      )
      
      # Compare these two!
      print("Rejection rate from power calculator = ", calculated_power)
      print("Simulated rejection rate using p-values = ", simulated_power)
      ```

      ----

      output:

      ```bash
      Rejection rate from power calculator =  0.6589069549458351
      Simulated rejection rate using p-values =  0.5334
      ```

      2. 

      ```python
      
      # Other way to increase power without changing the false positive rate is to increasing the size of the samples.
      # Or we can also decreasing the effect size to enlarge the value of power.
      # nobs means the size of the treatment group = the control group (share_treatment=0.5)
      nobs = power.tt_ind_solve_power(effect_size = ATE/sigma, alpha =0.05, power=0.8 )
      
      print (nobs)
      
      B = 5000
      N = 1500
      ATE = 0.15
      expectation_Y0 = 3
      sigma = 1
      alpha = 0.05
      # expectation_Y0, ATE, share_treatment, n, B, sigma, alpha
      simulation_results_new = get_simulation_results(B = B, n = N, expectation_Y0 = expectation_Y0, ATE = ATE, share_treatment = 0.5, sigma = sigma, alpha = alpha)
      
      simulated_power = simulation_results_new['p_reject']
      # Example of how to calculate the power for a given point.
      calculated_power = power.tt_ind_solve_power(
          effect_size=ATE / sigma,
          alpha=0.05,
          nobs1=N / 2,
          ratio=1.0,
      )
      
      # Compare these two!
      print("Rejection rate from power calculator = ", calculated_power)
      print("Simulated rejection rate using p-values = ", simulated_power)
      ```

      ---

      output 

      ![image-20200210132458838](C:\Users\28260\AppData\Roaming\Typora\typora-user-images\image-20200210132458838.png)

   -  Other way to increase power without changing the false positive rate is to increasing the size of the samples or we can also decreasing the effect size to enlarge the value of power.

   

<h2 
    style="text-align: center;" markdown="1"> Bonus
</h2>

1. 

- need the `math` package to run the following chunk of code

```python
# nobs means the size of the treatment group therefore the control group = 9*nobs (share_treatment=0.1)
import math

nobs = power.tt_ind_solve_power(effect_size = ATE/sigma, alpha =0.05, power=0.8, ratio=9 )
print(nobs)
N = math.ceil(nobs/0.1)
print(N)
# because nobs = size of treatment group

B = 5000
ATE = 0.15
expectation_Y0 = 3
sigma = 1
alpha = 0.05
# expectation_Y0, ATE, share_treatment, n, B, sigma, alpha
simulation_results_new = get_simulation_results(B = B, n = N, expectation_Y0 = expectation_Y0, ATE = ATE, share_treatment = 0.1, sigma = sigma, alpha = alpha)

simulated_power = simulation_results_new['p_reject']
# Example of how to calculate the power for a given point.
calculated_power = power.tt_ind_solve_power(
    effect_size=ATE / sigma,
    alpha=0.05,
    nobs1=N/10,
    ratio=9,
)

# Compare these two!
print("Rejection rate from power calculator = ", calculated_power)
print("Simulated rejection rate using p-values = ", simulated_power)
```

---

output:

![image-20200210133122255](C:\Users\28260\AppData\Roaming\Typora\typora-user-images\image-20200210133122255.png)



2. 

```python
alpha = 0.01
nobs = power.tt_ind_solve_power(effect_size = ATE/sigma, alpha =alpha, power=0.8, ratio=9 )
print(nobs)
N = math.ceil(nobs/0.1)
print(N)
# because nobs = size of treatment group

B = 5000
ATE = 0.15
expectation_Y0 = 3
sigma = 1
# expectation_Y0, ATE, share_treatment, n, B, sigma, alpha
simulation_results_new = get_simulation_results(B = B, n = N, expectation_Y0 = expectation_Y0, ATE = ATE, share_treatment = 0.1, sigma = sigma, alpha = alpha)

simulated_power = simulation_results_new['p_reject']
# Example of how to calculate the power for a given point.
calculated_power = power.tt_ind_solve_power(
    effect_size=ATE / sigma,
    alpha=alpha,
    nobs1=N/10,
    ratio=9,
)

# Compare these two!
print("Rejection rate from power calculator = ", calculated_power)
print("Simulated rejection rate using p-values = ", simulated_power)
```

---

output 

![image-20200210134125174](C:\Users\28260\AppData\Roaming\Typora\typora-user-images\image-20200210134125174.png)



The key trade-offs we are making with the product manager is that our aim is to make the A/B test successfully, we have to maintain a power larger than 0.8. In order to make it, we have to choose between false positive rate, size of the samples or effect size. 