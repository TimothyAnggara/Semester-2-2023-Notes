## Types of Errors

Lets explore the types of problems with a case study in breast cancer. In 1000 women, 10 women have breast cancer and 990 women do not have breast cancer

Out of the 10 women that have cancer, 9 test positive and 1 and tests negative.

Out of the 990 women that have cancer, 99 test positive and 891 test negative

### Test Results vs Actual Status
In general, **positive** = identified and **negative** = rejected
Therefore:
- **True Positive** = correctly identified
	- Mammogram returns a positive test result AND having breast cancer
- **False Positive** = incorrectly identified
	- Mammograms returns a positive test result AND not having breast cancer
- **True Negative** = correctly rejected
	- Mammogram returns a negative test result AND not having breast cancer
- **False Negative** = incorrectly rejected
	- Mammogram returns a negative test result AND having breast cancer

### Extra Notation
- $D^+$ - Event that an individual has a particular disease
	- **Prevalence** is the marginal probability of disease, $P(D^+)$
- $D^-$ - Event that an individual does not have a particular disease
- $S^+$ - Positive screen test result or a symptom being present
- $S^+$ - Negative screening test result or no symptom
![[Pasted image 20230808160418.png]]
![[Pasted image 20230808160934.png]]

- **False Negative Rate** - $P(S^- | D^+) = \frac{c}{a+c}$  
		- Testing negative given that you tested positive
- **False Positive Rate** - $P(S^+ | D^-) = \frac{b}{b+d}$
	- Testing positive given that you
- **Sensitivity / Recall** - $P(S^+ | D^+) = \frac{a}{a+c}$
	- Probability that the test is positive given that the subject actually has the disease
- **Specificity** - $P(S^- | D^-) = \frac{d}{b+d}$
	- Probability that the test is negative given that the subject does not have the disease
- **Positive Predictive Value / Precision** - $P(D^+ | S^+) = \frac{a}{a+b}$
	- Probability that the subject has the disease given that the test is positive
- **Negative Predictive Value** - $P(D^- | S^-) = \frac{d}{c+d}$
	- Probability that the subject does not have the disease given that the test is negative
- **Accuracy** -  $\frac{a+d}{a+b+c+d}$
## Conditional Probability
- Let $B$ be an event such that $P(B) > 0$
- The conditional probability of an event A given that B has occurred is:
$$
P(A|B) = \frac{P(A\cap B)}{P(B)}
$$
- Rearrange this equation we can also state that:
$$
P(A \cap B) = P(A|B)P(B)
$$
$$
P(A \cap B) = P(B|A)P(A)
$$

### Bayes' Rule
Bayes' rule allows us to reverse the conditioning set provided that we know some marginal probabilities
- Basically allows us to revert from $P(A|B) \rightarrow P(B|A)$

$$
P(B|A) = \frac{P(B \cap A)}{P(A)} = \frac{P(A|B)P(B)}{P(A|B)P(B) + P(A|B^c)P(B^c)}
$$

Note that:
- $B^c$ is the event that B doesn't occur 
- We used the **law of total probability** to find P(A)
$$
P(A) = P(A \cap B) + P(A \cap B^c) = P(A|B)P(B) + P(A|B^c)P(B^c)
$$
### Efficacy of HIV Test
A study comparing the efficacy of HIV tests, has concluded that the test have a **sensitivity** of 99.7% and a **specificity** of 98.5%. Suppose that a subject, from a population with a 0.1% **prevalence** of HIV receives a positive test result. What is the predictive value?
$$
Sensitivity=P(S^+|D^+) = 0.997
$$
$$
Specificity = P(S^-|D^-) = 0.985
$$
$$
Prevelance = P(D^+) = 0.01
$$
$$
Positive \space Predictive \space Value = \frac{P(S^+|D^+)P(D^+)}{P(S^+|D^+)P(D^+) + P(S^+|D^-)P(D^-)}
$$
$$
PPV = \frac{0.997 \times 0.01}{0.997 \times 0.01 + P(S^+|D^-)P(D^-)}
$$
$$
P(D^-) = 1 - P(D^+) = 1-0.001 = 0.999
$$
$$
P(S^+|D^-) = 1 - P(S^-|D^-) = 1-0.985 = 0.015
$$

$$
PPV = \frac{0.997 \times 0.01}{0.997 \times 0.01 + 0.015 \times 0.999} = 0.062
$$


### NIPT Example (Do it later today)
## Evaluating Classification Models


