## Two-factor ANOVA
In some experiments there may be more than one factor that may affect the response. In other words there may be multiple independent variables that affect the dependent variable
- Effects of diet and exercise on weight loss
	- Diet and exercise - independent variable
	- Weight loss - dependent variable
When doing a Multi-factor ANOVA, we are interested  whether:
- Each factor has an effect
- Effect of one factor is the same across all levels of the other factors

For the purpose of this unit, we shall focus on the case of **two factors**
### General Setup
We will have observations:
$$
\{y_{ijk}\colon\ i=1,\ldots,a;\ j=1,\ldots,b;\ k=1,\ldots,n\},
$$
- Where $y_{ijk}$ is the $k$-th observations receiving the treatment combinations of level $i$ of factor A and level $j$ of factor B
- There are $n$ observations receiving each treatment combinations
- The full model is that $y_{ijk}$ is the value taken by $Y_{ijk}\sim \mathcal{N}(\mu_{ij},\sigma^2)$
## Examples
### Poisons and Antidotes
A famous paper analyzed an experiment where one of each of 3 poisons and 4 antidotes were administered to a sample of 4 animals, giving 48 observations all together
- Response was survival time and in the study they took the reciprocal of the survival time as an appropriate response of the poison
The aim of the study was to determine how each antidote affected survival in the presence of each poison
```
# install.packages("BHH2")
data("poison.data", package = "BHH2")
# rename treat as antidote to avoid confusion with the general term "treatment"
poison.data = poison.data |> 
  rename(antidote = treat)
glimpse(poison.data)
```
![[Pasted image 20231002114559.png]]
```
poison.data = poison.data |> 
  mutate(inv_survival = 1/y)
poison.data |> ggplot() + 
  aes(x = poison, y = inv_survival) + 
  geom_boxplot() + 
  theme_classic(base_size = 30) + 
  facet_wrap(~antidote, ncol = 4) + 
  labs(y = "1/Survival time")
```
![[Pasted image 20231002115000.png]]
### Paper Planes
Results from an experiment where students launched paper planes in a controlled environment, controlling for various factors, including
- Paper quality with quality 1 (80gsm) and quality 2 (50gsm)
- Plane design with design 1 (High Performance) and design 2 (Simple)
```
planes = read_tsv("https://raw.githubusercontent.com/DATA2002/data/master/planes.txt")
glimpse(planes)
```
![[Pasted image 20231002115237.png]]
```
planes = planes |> 
  mutate(
    Paper = case_when(
      Paper == 1 ~ "80gsm",
      Paper == 2 ~ "50gsm"
    ),
    Plane = case_when(
      Plane == 1 ~ "HiPerf",
      Plane == 2 ~ "Simple"
    )
  )
```
![[Pasted image 20231002115247.png]]
```
p1 = ggplot(planes, aes(x = Plane, y = Distance)) + 
  geom_boxplot() + labs(y = "Distance (mm)") 
p2 = ggplot(planes, aes(x = Paper, y = Distance)) + 
  geom_boxplot() + labs(y = "Distance (mm)") 
gridExtra::grid.arrange(p1, p2, ncol = 2)
p3 = p1 + facet_wrap(~ Paper) + coord_flip()
p4 = p2 + facet_wrap(~ Plane) + coord_flip()
gridExtra::grid.arrange(p3, p4, ncol = 1)
```
![[Pasted image 20231002120425.png]]
### Various Questions
We can ask different questions about the structure of $\mu_{ij}$
- $H_0: \mu_{ij} = \mu$ 
	- A straight 1-way ANOVA between all treatment combinations
- $H_0: \mu_{ij} = \mu_i$ for all $j$
	- Factor B has no effect
- $H_0: \mu_{ij} = \mu_j$ for all $i$
	- Factor A has no effect
We can also ask if an **interaction** is present
- $H_0: \mu_{ij} = \mu + \alpha_i + \gamma_j$ for some constants $\mu, \alpha_i, \gamma_j$ so that the two factors combine additively
	- If this is true, then the adustment for each level of factor A is the same within each level of factor B
	- If this is not true, then we say there is some interaction between factors A and B
- This test is known as a "*test of no interaction*"

### THIS IS NOT A TWO WAY ANOVA
Note that although it looks similar to a two way ANOVA where we are adjusting for blocks, this is very different

When we are adjusting for blocks, the treatment and blocks combine additively and the null hypothesis states that all treatment affects are 0:
- $E(Y_{ij})=\mu_{ij}=\mu+\alpha_i+\beta_j$
- $H_0: \mu_{ij}=\mu+\beta_j$
- Note: $\beta_j$'s are not treatment effects but are block effects

In this scenario:
- The full model is: $E(Y_{ijk})=\mu_{ij}$
- $H_0: \mu_{ij}=\mu+\alpha_i+\gamma_j$ | Factor A and factor B effects combine additively
- Both $\alpha_i$ and $\gamma_j$ are related to treatment effects
## Some Theory
Some variables:
- $\mu = \bar\mu_{\bullet\bullet}$ (Overall Mean)
- $\alpha_i = \bar{\mu}_{i\bullet}-\bar\mu_{\bullet\bullet}$ (Main effect due to $i$-th level of factor A)
- $\gamma_j = \bar{\mu}_{\bullet j}-\bar\mu_{\bullet\bullet}$ (Main effect due to $j$-th level of factor B)
- Interaction effect between level $i$ of factor A and level $j$ of factor B:
	- $(\alpha\gamma)_{ij} = \mu_{ij}-(\mu+\alpha_i+\gamma_j) =\mu_{ij}-\bar \mu_{i\bullet} -\bar\mu_{\bullet j}+\bar \mu_{\bullet\bullet}$
We may write:
$$
\mu_{ij}=\mu + \alpha_i + \gamma_j + (\alpha\gamma)_{ij}\ .
$$
### Main effects: Interpretation
Each $\alpha_i$ is in fact a contrast in the treatment combination group which means that it measures in some overall sense how the means for level $i$ of factor A differ from the overall average
- In the same way, each $\gamma_j$ is a contrast that compares level $j$ of factor B to the overall average
A test for no interaction can be formulated as the following null hypothesis:
$$
H_0\colon\ (\alpha\gamma)_{ij}=0 \ \text{ for all }i,j.
$$
### Constraints: Main effects degrees of Freedom
- Note that these "effects" must satisfy certain constraints where:
$$
\sum_{i=1}^a\alpha_i = \sum_{j=1}^b\gamma_j=0\,.
$$
Hence,
- $\alpha-1$ "free" $\alpha_i$'s and
- $b - 1$ "free" $\gamma_j$'s
Henceforth we can say that:
- There is $a-1$ degrees of freedom for factor A main effects
- There is $b-1$ degrees of freedom for factor B main effects
### Fitted Values and Residuals for Each Model
We have two models:
- The full model where $\mu_{ij}$ is "unrestricted"
- The "no interaction" hypothesis where $\mu_{ij} = \mu + \alpha_i + \gamma_j$
And in each of these models, each observation, $y_{ijk}$ is decomposed into two parts:
- A fitted value: $\hat{\mu_{ij}}$
- A residual: $y_{ijk} - \bar{\mu_{ij}}$
Under the full model, $\hat{\mu}_{ij}=\bar y_{ij\bullet}$, the mean of the $(i,j)$-th treatment combination
Under the no interaction model, the fitted value is:
$$
\begin{align}
\hat\mu_{ij} & =\hat\mu + \hat{\alpha}_i+ \hat{\gamma}_j \\
& = \bar y_{\bullet\bullet\bullet}+(\bar y_{i\bullet\bullet}-\bar y_{\bullet\bullet\bullet}) + (\bar y_{\bullet j\bullet}-\bar y_{\bullet\bullet\bullet}) \\
& = \bar y_{i\bullet\bullet}+\bar y_{\bullet j \bullet}- \bar y_{\bullet\bullet\bullet}
\end{align}
$$
### Residual Sum of Squares For Each Model
- For the full model, the $(i,j,k)$-th residual is $y_{ijk}-\bar y_{ij\bullet}$ and the residual sum of squares is:
$$
\sum_{i=1}^a\sum_{j=1}^b\sum_{k=1}^n \left( y_{ijk}-\bar y_{ij\bullet} \right)^2\,.
$$
- For the no interaction model, the $(i,j,k)$-th residual is $y_{ijk}-\bar y_{i\bullet\bullet}-\bar y_{\bullet j\bullet}+\bar y_{\bullet\bullet\bullet}$ and the residual sum of squares is:
$$
\sum_{i=1}^a\sum_{j=1}^b\sum_{k=1}^n \left( y_{ijk}-\bar y_{i\bullet\bullet}-\bar y_{\bullet j\bullet}+\bar y_{\bullet\bullet\bullet} \right)^2\,.
$$
$$
\scriptsize{
\begin{aligned}
\text{Total SS} & = \sum_{i=1}^a\sum_{j=1}^b\sum_{k=1}^n(y_{ijk}-\bar y_{\bullet\bullet\bullet})^2\\
& = \sum_{i=1}^a\sum_{j=1}^b\sum_{k=1}^n\left[(\bar y_{i\bullet\bullet}-\bar y_{\bullet\bullet\bullet}) + (\bar y_{\bullet j\bullet}-\bar y_{\bullet\bullet\bullet})+(y_{ijk}-\bar y_{i\bullet\bullet} - \bar y_{\bullet j\bullet} +\bar y_{\bullet\bullet\bullet})\right]^2\\
& = \underbrace{\sum_{i=1}^abn (\bar y_{i\bullet\bullet}-\bar y_{\bullet\bullet\bullet})^2}_{\text{Factor A Sum Sq.}} + \underbrace{\sum_{j=1}^ban (\bar y_{\bullet j\bullet}-\bar y_{\bullet\bullet\bullet})^2}_{\text{Factor B Sum Sq.}} + \underbrace{\sum_{i=1}^a\sum_{j=1}^b\sum_{k=1}^n (y_{ijk}-\bar y_{i\bullet\bullet} - \bar y_{\bullet j\bullet} +\bar y_{\bullet\bullet\bullet})^2}_{\text{no interaction model Res Sum Sq.}} + \text{ cross-product terms (all 0)}\\
& = \sum_{i=1}^abn (\bar y_{i\bullet\bullet}-\bar y_{\bullet\bullet\bullet})^2 + \sum_{j=1}^ban (\bar y_{\bullet j\bullet}-\bar y_{\bullet\bullet\bullet})^2 + \sum_{i=1}^a\sum_{j=1}^b\sum_{k=1}^n \left[(\bar y_{ij\bullet}-\bar y_{i\bullet\bullet} - \bar y_{\bullet j\bullet} + \bar y_{\bullet\bullet\bullet})+(y_{ijk}-\bar y_{ij\bullet})\right]^2\\
& = \sum_{i=1}^abn (\bar y_{i\bullet\bullet}-\bar y_{\bullet\bullet\bullet})^2 + \sum_{j=1}^ban (\bar y_{\bullet j\bullet}-\bar y_{\bullet\bullet\bullet})^2\\
& \ \ \  + \underbrace{ \sum_{i=1}^a\sum_{j=1}^bn (\bar y_{ij\bullet}-\bar y_{i\bullet\bullet} - \bar y_{\bullet j\bullet} + \bar y_{\bullet\bullet\bullet})^2}_{\text{Interaction Sum Sq.}}  + \underbrace{\sum_{i=1}^a\sum_{j=1}^b\sum_{k=1}^n (y_{ijk}-\bar y_{ij\bullet})^2}_{\text{full model Res Sum Sq.}} + \text{ a cross-product term which is zero}\,.
\end{aligned}
}
$$
### $F$-Test For No Interaction
- Under the full model, the **residual sum of squares**:
$$
\sum_{i=1}^a\sum_{j=1}^b\sum_{k=1}^n (Y_{ijk}-\bar Y_{ij\bullet})^2\sim \sigma^2\chi^2_{ab(n-1)}\,,
$$
Since:
- The total sample size is $N = abn$
- Number of groups is $g = ab$, hence
- The degrees of freedom for residuals is $N-g = abn - ab = ab(n-1)$

- It turns out that under the no interaction model ($H_0$),  the random variable version of the interaction sum of squares is:
$$
\sum_{i=1}^a\sum_{j=1}^bn (\bar Y_{ij\bullet}-\bar Y_{i\bullet\bullet} -\bar Y_{\bullet j\bullet} +\bar Y_{\bullet\bullet\bullet})^2 \sim \sigma^2 \chi^2_{(a-1)(b-1)}
$$
- The $F$-statistic for testing the null hypothesis of no interactions is:
$$
\small{
\begin{aligned}
\frac{\text{Interaction Mean Square}}{\text{(full model) Residual Mean Square}} &= \frac{(\text{Int. SS})/[(a-1)(b-1)]}{\text{(full model Residual SS)}/[ab(n-1)]}\\&\sim F_{(a-1)(b-1),ab(n-1)}
\end{aligned}
}
$$
- IF THE NULL HYPOTHESIS OF NO INTERACTION IS TRUE
## Back to Examples
### Poisons and Antidotes
Recall that we have two factors, poisons with 3 levels and antidotes with 3 levels
```
levels(poison.data$poison)
# "I", "II", "III"
levels(poison.data$antidote)
# "A", "B", "C", "D"
```
What we could do here, is fit the model to a "big one-way ANOVA" where we create a new factor which has every possible combination of the `poison`:`antitode` and then we do a regular ANOVA with it
```
poison.data$poison:poison.data$antidote
```
![[Pasted image 20231002181519.png]]
```
a1 = aov(inv_survival ~ poison:antidote, data = poison.data)
anova(a1)
```
![[Pasted image 20231002181755.png]]
### A Better Approach
A much better way to proceed with the two-factor ANOVA is by using an appropriate formula:
- We fit `poison:antidote` after fitting the main effects:
```
a3 = aov(inv_survival ~ poison + antidote + poison:antidote, 
         data = poison.data)
summary(a3)
```
![[Pasted image 20231002181928.png]]
- The sum of squares for `poison:antidote` only contains the variation explained by treatments and *not already explained by main effects*
- This provides a convenient partition of the "big one-way ANOVA" **treatment sum of squares** (11 df) into 3 contributions:
	- 2 df for `poison` main effect
	- 3 df for `antidote` main effect
	- 6 df for the interaction effect
### Paper Planes
Lets perform a similar analysis on the paper planes data, lets first start with the "big one-way ANOVA"
```
summary(aov(Distance ~ Paper:Plane, data = planes))
```
![[Pasted image 20231002182452.png]]
And now lets split the treatment sum of squares into their main effects and interactions:
```
plane_aov = aov(Distance ~ Paper * Plane, data = planes)
summary(plane_aov)
```
![[Pasted image 20231002182522.png]]
- We can see that there is a strong interaction effect
## Interaction plots
We can the interactions between the factors using an interaction plot or a trace plot
- Basically, we take the mean of each treatment combination and draw those means in a scatterplot and draw lines between them
- If there is no interactions between factor A and factor B, the lines should be roughly parallel
- If there is an interaction then we would expect to see intersections or deviate from paralleism
### Poison / Antidotes Interaction Plots
```
sum_dat = poison.data |> group_by(poison, antidote) |> 
  summarise(mean = mean(inv_survival),
            n = n())

sum_dat |> 
  ggplot(aes(x = antidote, y = mean,
             group = poison, 
             linetype = poison,
             colour = poison)) + 
  geom_point(size = 4) +
  geom_line(size = 1.5) + 
  theme_classic(base_size = 36)
sum_dat |>
  ggplot(aes(x = poison, y = mean,
             group = antidote, 
             linetype = antidote,
             colour = antidote)) + 
  geom_point(size = 4) +
  geom_line(size = 1.5) + 
  theme_classic(base_size = 36)
```
![[Pasted image 20231002183243.png]]
### Paper Planes Interaction Plots
```
plane_sum_dat = planes |> 
  group_by(Plane, Paper) |> 
  summarise(mean = mean(Distance))
  
plane_sum_dat |>
  ggplot(aes(x = Paper, y = mean,
             group = Plane, 
             linetype = Plane)) + 
  geom_point(size = 4) +
  geom_line(size = 1.5) + 
  theme_classic(base_size = 36)
  
plane_sum_dat |> 
  ggplot(aes(x = Plane, y = mean,
             group = Paper, 
             linetype = Paper)) + 
  geom_point(size = 4) +
  geom_line(size = 1.5) + 
  theme_classic(base_size = 36)
```
![[Pasted image 20231002183453.png]]
## Post Hoc Test
So if we reject the null hypothesis, we may want to zoom in and say which level A are different from one another or which factors of level B are different from one another. 
- We can use the `emmeans` package to do this for us
### Comparing Poisons
We determined that there is no interactions between `poison` and `antidote` so we can go ahead and compare different `poison` to each other and `antidotes` to each other
```
a2 = aov(inv_survival ~ poison + antidote, data = poison.data)
emmeans(a2, ~ poison) |> contrast(method = "pairwise", adjust = "none")
```
![[Pasted image 20231002184021.png]]
```
emmeans(a2, ~ antidote) |> contrast(method = "pairwise", adjust = "none")
```
![[Pasted image 20231002184029.png]]
### Assessing Significance
- We have a few options when it comes to comparing these:
	- Bonferroni
	- Tukey's Method
	- Scheffe's method

### Bonferroni Method
```
p_emm = contrast(emmeans(a2, ~poison), method = "pairwise", adjust = "bonferroni")
p_emm
```
![[Pasted image 20231002184405.png]]
```
a_emm = contrast(emmeans(a2, ~antidote), method = "pairwise", adjust = "bonferroni")
a_emm
```
![[Pasted image 20231002184414.png]]
The `emmeans` packages isn't doing the Bonferroni method quite correctly because we actually did 9 test, 3 test for poison and 6 test for antidotes. So we actually have to multiply the p-value by 9.
- To fix this we can actually tell the `emmeans` that  we are actually looking at 9 test
```
pa_emm = update(p_emm + a_emm)
pa_emm
```
### Paper Planes
