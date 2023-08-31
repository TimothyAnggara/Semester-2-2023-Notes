## Motivating Example

### Asthma and Hay fever
A large group of infants with mild respiratory problems, but asthma free is split into those with a family of hay fever and those without

Random samples of 85 from the first group and 405 from the second group are selected for a special study. From these, the number diagnosed with asthma by the age of 12 are 25 and 70 respectively

|      | Asthma | No Asthma |     |
| ------------ | -------- | -------- | --- |
| Hay Fever    | 25       | 60       |  85   |
| No Hay Fever | 70       | 335      |   405  |
|              | 95       | 395      |     |

### Hodgkin's disease and tonsillectomies
We have another study where **101 patients suffering from Hodgkin's disease** and a comparable control group of **107 non-Hodgkin's patients**

The study was interested in the effect of tonsil tissue as a barrier to Hodgkin's disease and they found that there has been 67 patients with the disease + tonsillectomies and 43 patients without the disease + tonsillectomies

|                       | With Hodgkin's | Without Hodgkins |     |
| --------------------- | -------------- | ---------------- | --- |
| With Tonsillectomy    | 67             | 43               | 110 |
| Without Tonsillectomy | 34             | 64               | 98  |
|                       | 101            | 107              |     |

### Notation
Lets outline some notation that we will be using for the lecture regarding conditional probabilities
- $D^+$ - Having the disease
- $D^-$ - Not having the disease
- $R^+$ - Having the risk factor
- $R^-$ - Not having the risk factor

## Prospective Studies
A prospective study involves one or more groups of subjects (cohorts) are being followed over time to observe any changes with respect to a disease or outcome.
- Based on subjects who are initially "disease-free" and classified by presence or absence of a "risk-factor"

These observations are then used to determine what exposure characteristics (risk factors) are associated with the out
- A random sample from each group are followed until eventually classified by disease outcome

### Prospective Studies Example
In a weekend beach volleyball tournament, a prospective study was conducted to assess the impact of sun exposure on skin damage. Players from one team wore waterproof, SPF35 sunscreen, while the other team did not wear any sunscreen. 

After the tournament, skin samples were collected from both teams and analyzed for texture, sun damage, and burns. The comparison between the two groups (cohorts) revealed a significant difference in terms of skin damage, demonstrating the protective effect of the sunscreen.

This study's design illustrates the application of a prospective cohort study by selecting two groups with different exposures (sunscreen use), following them over a short time (the tournament weekend), and then analyzing the outcomes (skin damage) to identify associations between exposure and outcome.

### Prospective Study - Asthma
A large group of infants with mild respiratory problems, but asthma free is split into those with a family history of hay fever,$R^+$, and those without, $R^-$.

Random samples of 85 from the first group and 405 from the second group are selected for a special study. From these, the number diagnosed with asthma by the age of 12, $D^+$, are 25 and 70 respectively

|                    | Asthma $D^+$ | No Asthma $D^-$ |     |
| ------------------ | ------------ | --------------- | --- |
| Hay Fever $R^+$    | 25           | 60              | 85  |
| No Hay Fever $R^-$ | 70           | 335             | 405 |
|                    | 95           | 395             |     |

- Risk factor - Family history of hay fever
- Disease - Asthma by age 12


### Retrospective Studies
A study that compares patients who have a disease or outcome of interest with patients who do not have the disease or outcome.

The study then looks back retrospectively to compare how frequently the exposure to a risk factory is present in each group to determine the relationship between the risk factory and the disease

### Retrospective Studies - Fictitious Example
A retrospective study was conducted to investigate the effectiveness of zinc oxide (a traditional, non-absorbent sunscreen) in preventing sunburns leading to skin cancer, compared to absorbent sunscreen lotions.

Researchers compared two groups of former lifeguards: one group that had developed cancer on their cheeks and noses (cases) and another without this type of cancer (controls). 

Participants were asked to recall their previous use of zinc oxide or absorbent sunscreen lotions. The study's retrospective nature means that it looked back at past behaviors and outcomes, relying on participants' memories rather than observing them prospectively over time.

The main focus of this study is to determine whether a particular exposure (using zinc oxide sunscreen) has a specific outcome (prevention of skin cancer) by analyzing data from the past. It illustrates the typical structure of a retrospective study, utilizing existing cases and controls and relying on participants' recall of past exposures.

### Retrospective Study - Hodgkin's Disease
We have another study where **101 patients suffering from Hodgkin's disease**,$D^+$$, and a comparable control group of **107 non-Hodgkin's patients**, $D^-$

The study was interested in the effect of tonsil tissue as a barrier to Hodgkin's disease and they found that in the $D^+$ group, there had been 67 tonsillectomies, $R^+$, and for the corresponding figure for $D^-$ was 43

|                             | With Hodgkin's $D^+$ | Without Hodgkins $D^-$ |     |
| --------------------------- | -------------------- | ---------------------- | --- |
| With Tonsillectomy $R^+$    | 67                   | 43                     | 110 |
| Without Tonsillectomy $R^-$ | 34                   | 64                     | 98  |
|                             | 101                  | 107                    |     |
- Risk Factor - Tonsillectomy
- Disease - Hodgkin's disease

## Estimating Population Proportions

Imagine you have a large population containing objects / individuals of two different types, type 0 and type 1 and we want to estimate the overall proportion of type 1 but the population is too large

We can take a random sample from the population and then we can use the sample's proportion of type 1 as an estimate of the population's proportion of type 1

We can take a random sample from the population and we can estimate P(A) using the observed sample proportion with attribute A

If we can take a random sample from the subpopulation defined by B, we can estimate P(A|B) using the observed sample subpopulation with attributed A

### Application to Prospective and Retrospective Studies

In both kinds of study we have:
- A population
- A subpopulation determined by a risk factor $R^+$ with a complementary subpopulation $R^-$
- A subpopulation determined by developing the disease $D^+$

The main difference between prospective and retrospective studies are which (sub)populations we can sample from
- Either we sample from the subpopulation $D$ or $R$
- 

### Prospective Study
In a prospective study, we take two random samples from the risk factor group $R^+$ and another from $R^-$. 

After we taken the sample, we wait to see how many in each group develops the disease, hence we can only estimate:
- $P(D^+|R^+)$ and $P(D^-|R^-)$
### Retrospective Study
In a retrospective study, we take two random sample from the disease group, $D^+$ and from the non-disease group $D^-$

We then look back to see how many in each group were exposed to the risk factor, hence we can only estimate:
- $P(R^+|D^+)$ and $P(R^-|D^-)$


## Relative Risk
There are many ways to measure associated between a risk factor and the disease outcome and how the data is being **sampled** will greatly impact which methods are applicable and interpretable

**Relative risk** is defined as a **ratio of two conditional probabilities**

$$
RR = \frac{P(D^+|R^+)}{P(D^+|R^-)}
$$
	- RR = 1 : There is no difference between the two groups
- RR < 1 : Implies the disease is less likely to occur in groups with the risk factor
- RR > 1 : Implies the disease is more likely to occur in the group with the risk factor

### Relative Risk - Prospective Studies
  |  | $D^+$ |  $D^-$   |         |
  | ----- | ----- | --- | ------- |
  | $R^+$ | a     | b   | a+b     |
  | $R^-$ | c     | d   | c+d     |
  |       | a+c   | b+d | a+b+c+d |

Given a prospective study, we can estimate these:
- $P(D^+|R^+) = \frac{a}{a+b}$
- $P(D^+|R^-) = \frac{c}{c+d}$
- Relative Risk : $\hat{RR} = \frac{{P(D^+|R^+)}}{P(D^+|P^-))} = \frac{a(c+d)}{c(a+b)}$

### Relative Risk - Retrospective Studies
  |  | $D^+$ |  $D^-$   |         |
  | ----- | ----- | --- | ------- |
  | $R^+$ | a     | b   | a+b     |
  | $R^-$ | c     | d   | c+d     |
  |       | a+c   | b+d | a+b+c+d |
In a retrospective study, we identify two groups, $D^+$ and $D^-$ and retrospectively assess each group their risk status, $R^+$ and $R^-$

Due to the design of this study, we cannot extract any information about the incidence of $D$ in the population because the cases of $D^+$ and $D^-$ was decided by the investigator



### Aspirin (Relative Risk)
Steering Committee of the Physicians' Health Study Research Group provide data on a 5 year (blind) study into the effect of taking aspirin every second day on the incidence of heart attacks

  |               | Mycocaridal Infection $D^+$ | No Mycocardial Infection $D^-$ |        |
  | ------------- | --------------------------- | ------------------------------ | ------ |
  | Aspirin $R^+$ | 104                         | 10,933                         | 11,037 |
  | Placebo $R^-$ | 189                         | 10,845                         | 11,034 |
  |               | 293                         | 21,778                         | 22,071 |

$$
P(D^+|R^+) = \frac{104}{10,933 + 104} = 0.0094
$$
$$
P(D^+ | R^-) = \frac{189}{10,845 + 189}
$$
$$
\hat{RR} = \frac{0.0094}{0.0171} = 0.55
$$

## Odds Ratio
Odds are a ratio of probabilities, the odds are used as an alternative way of measuring the likelihood of an event occurring

If the probability of event A is P(A), the odds of event A is:
$$
O(A) = \frac{P(A)}{1-P(A)}
$$

In the risk/disease setting, the probability of disease %R^+$ patients is $P(D^+|R^+)$, hence the odds:
$$
O(D^+|R^+) = \frac{P(D^+|R^+)}{1-P(D^+|R^+)} = \frac{P(D^+|R^+)}{P(D^-|R^+)}
$$
### Equivalent definitions of odds ratio
The ratio of the odds of a disease for $R^+$ patients to the corresponding odds for $R^-$ patients is the odds ratio, $OR$
$$
OR = \frac{O(D^+|R^+)}{O(D^+|R^-)} = \frac{P(D^+|R^+)}{P(D^-|R^+)} / \frac{P(D^+|R^-)}{P(D^-|R^-)}
$$

$$
OR = \frac{O(R^+|D^+)}{O(R^+|D^-)} = \frac{P(R^+|D^+)}{P(R^-|D^+)} / \frac{P(R^+|D^-)}{P(R^-|D^-)}
$$
We can see that the ratio for the odds ratio are identical and can be found from both prospective and retrospective studies

OR = 1 if and only if $D$ and $R$ are independent and Large odds ratio ($OR > 1$) implies increased risk of disease and small odd ratios ($OR < 1$) implies decreased risk of disease

## Standard Errors and Confidence Intervals for Odds Ratios

The **odds ratio** estimator, $OR$, has a skewed distribution on $(0, \infty)$, with the neutral value being 1

The **log odds ratio** estimator, $log(OR)$, has more symmetric distribution centered at 0 if there is no difference between the two groups

Asymptotic **standard error** for the log odds ratio estimator, $log(\hat{OR})$, is 
$$
SE(log(\hat{OR})) = \sqrt{\frac{1}{a} + \frac{1}{b} +  \frac{1}{c} + \frac{1}{d} }
$$
### Standard Errors and Confidence Intervals

Large sample $95\%$ confidence interval for log $\theta$ is approximately
$$
log(\hat{OR}) \pm 1.96 \times SE(log\hat{(OR)})
$$

Exponentiating the lower and upper ends of the log(OR) confidence interval gives us an approximate a confidence interval for the odds-ratio,
$$
[exp(log(\hat{OR}) - 1.96SE(log(\hat{OR}))), exp(log(\hat{OR}) + 1.96SE(log(\hat{OR})))]
$$

This should only be applied if $a, b, c, d$ are reasonably large (so that the asymptotic hold)

### Aspirin Example
  |               | Mycocaridal Infection $D^+$ | No Mycocardial Infection $D^-$ |        |
  | ------------- | --------------------------- | ------------------------------ | ------ |
  | Aspirin $R^+$ | 104                         | 10,933                         | 11,037 |
  | Placebo $R^-$ | 189                         | 10,845                         | 11,034 |
  |               | 293                         | 21,778                         | 22,071 |

$$
Total = \hat{OR} = \frac{104 \times 10,845}{189 \times 10,933} = 0.55
$$
$$
log(\hat{OR}) = -0.6
$$
$$
SE(log(\hat{OR}))  = \sqrt{\frac{1}{104} + \frac{1}{189} +  \frac{1}{10933} + \frac{1}{10845} }
$$
A 95% confidence interval for the log odds-ratio is:
$$
-0.6 \pm 1.96 \times 0.12 = (-0.84, -0.36)
$$

A 95% confidence interval for the odds-ratio is:
$$
(e^{-0.84}, e^{-0.36}) = (0.43, 0.69)
$$