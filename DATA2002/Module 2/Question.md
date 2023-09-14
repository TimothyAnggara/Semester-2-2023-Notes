- the Confidence Interval can be only used for normal distribution?
- Permutation test only assumes that the observations $X_1, X_2, ..., X_n$, and $Y_1,Y_2,...,Y_n$ are exchangeable under $H_0$ that is, swapping labels on observations keeps the data just as likely as the original (???)
```
B = 10000 # number of permuted samples we will consider
permuted_heat = heat # make a copy of the data
t_null = vector("numeric", B) # initialise outside loop
for(i in 1:B) {
  permuted_heat$method = sample(heat$method) # this does the permutation
  t_null[i] = t.test(energy ~ method, data = permuted_heat)$statistic
}
mean(t_null>=t0_original) #0.0036
```
- (Why didn't we specify alternative = "greater" in t.test())
- If there was **no association** between microRNAs and Alzheimer's disease, we would see that our p-values follow a uniform distribution (why?)
- Help explain the basic idea of the Benjamini-Hochberg Procedure and the Bonferroni correction and how it works
