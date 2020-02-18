<h1 align="center"> Problem Set #3 </h1>

Creator: <font color="blue">Junjie Lei</font>

CreationDate: <font color="blue"> Feb_10</font>

Title: <font color="blue">Problem Sets #3 experiments and regressions </font>

Notes: [$Colab$](https://colab.research.google.com/drive/1xmZXb29jz1nrVD87hR1gbC4W5dVBUNZa#scrollTo=lxEmEbn7PNHP)​

---

<h2 align="center">In-class notebook exercises</h2>

<h3 align="center"> Replicate and simulate a real study </h3>

1.1.1

```python
print(r_col3.summary())
print(r_col4.summary())
```



1.1.2

- $\textit{Column 4}$  has more control variables

- to calculate the $\textit{R Square}$ 

  ```python
  print(r_col3.rsquared) ## --> 0.0671339542940943
  print(r_col4.rsquared) ## --> 0.10819457890249662
  ```

  $\textit{Column 4}$ has a higher R-squared, because it contains more control variables hence the model has more explanatory power; 

  

-  to calculate the $\textit{Std.ev}$

  ```python
  print(r_col3.bse.rem_any) ## --> 0.009153114318622113
  print(r_col4.bse.rem_any) ## --> 0.008949743622819572
  ```

  $\textit{Column 4}$ has a smaller a standard error on the estimated $\textit{ATE}$ ; 



1.1.3

<img src="C:\Users\28260\AppData\Roaming\Typora\typora-user-images\image-20200217225154463.png" alt="image-20200217225154463" style="zoom:100%;" />

- for the estimation of the effect of `rem _any` in the experiments; 

  the estimated coefficients and the standard error are listed as follow

  ```python
  print(r_col3.params.rem_any)  ## --> 0.03185505049068642
  print(r_col4.params.rem_any)  ## --> 0.03186131518614749
  print(r_sim_biv.params.d)     ## --> 0.031709816056386286
  print(r_sim_control.params.d) ## --> 0.03240621068111807
  ```

  - the estimated coefficients and the standard error from the simulation are pretty close to real data;



1.1.4

- set the `beta_hs = 0.35` for the simulation
- set `B = 1000` 

similar to the `compare_lpm_prop_test` function, we define a new function

```python
def new_simulation_finction(
  N = 10000,
  beta_hs = 0.35,
  ATE = 0.032
):
  grad_high_school = np.random.binomial(n=1, p=0.5, size=N)
  D = np.random.binomial(n=1, p=0.61, size=N)
  baseline_probability = 0.25 + beta_hs * grad_high_school
  Y0 = np.random.binomial(n=1, p=baseline_probability)
  Y1 = np.random.binomial(n=1, p=baseline_probability + ATE * D)
  dff = pd.DataFrame({
    'grad_high_school': grad_high_school,
    'd': D,
    'y0': Y0,
    'y1': Y1
  })
  dff['y'] = dff.eval("y1 * d + y0 * (1 - d)")
  regr = sfa.ols("y ~ d", dff).fit()
  a = regr.params['d']
  return a
```

and we loop it;

```python
B = 1000
res = pd.DataFrame([new_simulation_function() for i in np.arange(0, B)])
```

and we see the results; 

<img src="C:\Users\28260\AppData\Roaming\Typora\typora-user-images\image-20200217232821168.png" alt="image-20200217232821168" style="zoom:100%;" />



1.1.5

for this question, we replicate what we did from the previous simulation; 

and we plot the `params['d']` to show the estimated treatment effects;

<img src="C:\Users\28260\AppData\Roaming\Typora\typora-user-images\image-20200217233412617.png" alt="image-20200217233412617" style="zoom:50%;" /> <img src="C:\Users\28260\AppData\Roaming\Typora\typora-user-images\image-20200217233543211.png" alt="image-20200217233543211" style="zoom:50%;" />



1.1.6

from the model1 the $\textit{Std.ev} \rightarrow 0.009991$ 

from the model2 the $\textit{Std.ev} \rightarrow 0.009918$



1.1.7 

refer to the `Confidence Interval machine` 

```markdown
Left board of confidence lever for model 1 =  `0.03159373629542879`
Right board of confidence lever for model 1 =  `0.03284826370457121`
Left board of confidence lever for model 2 = `0.03114525827711666`
Right board of confidence lever for model 2 =  `0.032286741722883344`
```

and we count when the confidence intervals include the `true ATE` in each model

```python
k = 0
for i in np.arange(0, B):
  if ch[0][i] >= lboard1 and ch[0][i] <= rboard1:
    k = k+1
  else:
    k = k + 0
```

the output for `k` is $58$

---



<h3 align="center">Shoe technology experiment</h3>



1.2.1















































