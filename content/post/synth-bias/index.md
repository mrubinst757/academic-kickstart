---
title: The Linear Factor Model and the Bias of Synthetic Controls
date: 2020-05-24
math: true
diagram: false
---
At least some of the excitement about the Synthetic Controls method comes from the proof in Abadie et al. (2010), where the authors show that assuming the outcome model absent treatment follows a linear factor model, we can bound the bias of the estimator for a finite number of pre-treatment time-periods ($T_0$). A lot of interesting papers have been written this paper analyzing this bound under different assumptions (see, eg, Botosaru & Ferman (2019); Ferman & Pinto (2019)). I’m writing this to summarize some of my thoughts after thinking about this bound and to ask a few questions that hopefully people who know more than me might be able to answer. I don't think I'm going to get everything right here -- and if anyone who reads this notices any mistakes, or I’m missing some point, please reach out to me – I’d love to learn more.

As a brief background to motivate the bound derived in Abadiet et al (2010), the context for the synthetic controls literature is frequently a panel of data with units $j = 0, ..., J$ observed across time-periods $T = 0, ..., T\_0, ..., T$, where unit $j = 0$ is treated at time $T\_0 + 1$. We then posit the following model for the potential outcomes absent treatment:

$$
Y\_{it}^0 = \beta\_tX\_i + \lambda\_t\mu\_i + \epsilon\_{it}
$$

where $\lambda\_t$ is a $(1 x F)$ vector of factors and $\mu\_i$ is an $(F x 1)$ vector of factor loadings. I think people like this model because it generalizes the two-way fixed effects model, which becomes the special case of this model with two factors: $\lambda\_{t, 1} = 1$ for all periods (ie unit fixed-effects), and the second factor $\lambda\_{t, 2}$ has associated factor loadings $\mu\_{i, 1} = 1$ for all units. More generally, this model basically allows regions to experience common shocks in different ways, whereas the two-way fixed effects model makes the restriction that all common shocks are experienced the same way across regions.

Abadie et al (2010) then prove that the bias of the synthetic controls estimator is bounded in this setting. The proof relies on the existence of weights that exactly balances the means of the control group's covariates $X\_0$ and the pre-treatment outcomes $Y_0$ for $T_0$ time periods to the treated unit's covariate/pre-treatment outcome values. That is, letting $Z = (X, Y)$ we have a $(J x 1)$ vector of weights $w$ such that 

$$
Z\_0'w = Z\_1
$$

That's already a pretty strong condition, especially considering its uncommon that one gets exact mean-balancing weights in any given application (more on this later). Nevertheless, we carry on and assume that there are F factors where $\lambda\_{tf}$ are bounded by $\bar{\lambda}$. We also assume that the covariance matrix of these factors in the pre-treatment period $(\frac{1}{T\_0}\sum_{t = 1}^{T\_0}\lambda\_t'\lambda$) is non-singular, and that the minimum eigenvalue is $\bar{\zeta} > 0$. We also let $m\_{p, tj} = \mathbb{E}\mid\epsilon\_{jt}\mid^p$, $m\_{p,j} = \frac{1}{T\_0}\sum\_{t = 1}^{T\_0}m\_{p, tj}$, $\bar{m\_p} = \max\_{j = 1,...,J}m\_{p, j}$ (whew), and lastly we let $\sigma\_{jt} = \mathbb{E}(\epsilon\_{jt}^2)$, $\sigma\_j = \frac{1}{T\_0}\sum\_{t=1}^{T\_0}\sigma\_{jt}^2$, $\bar{\sigma} = \max\_{j = 1, ..., J} \sqrt{\sigma\_j^2}$.

The authors then show that the bias of this estimator is bounded by the following frightening expression:

$$
C\frac{\bar{\lambda}F}{\bar{\zeta}}\max(\frac{\bar{m\_p}^{\frac{1}{p}}}{T\_0^{1 - \frac{1}{p}}}, \frac{\bar{\sigma}}{T\_0^{\frac{1}{2}}})
$$ 

There are a couple of things worth noting here (and these things have been noted before). First of all we assumed that the maximum value of the unobserved factors is bounded across all time periods from $t = 1,..., T$. Imagine trying to run this algorithm on some exponentially increasing process, like, for instance, COVID-19 deaths. You may think that over some fixed time period the value of these factors are bounded, but this result suggests that the bias bound might be pretty bad when we have outcomes that increase exponentially over time (non-stationary outcomes). This suggests to me that analyzing a stationary outcome should be easier. 

Yet I just read Ferman & Pinto (2019), who I read as saying that asymptotically, matching on factor loadings with non-stationary outcomes is in general easier. In fact, they show that a demeaned version of the synthetic controls estimator (ie, subtract the pre-treatment mean for each unit) will exactly balance these factor loadings if weights that balance those factor loadings exist! This seems counterintuitive to me given the bound noted above; moreover, Cummins et al (2019) did some nice simulations on synthetic controls. In that paper they fix $T\_0$ and run simulations with different polynomial time trends, and the bias gets worse when adding polynomial factors for p > 1 (Figure 7). I’m not sure how to reconcile this, although Ferman & Pinto's result is asymptotic and relies on demeaning the outcomes (which, importantly, is a different identifying assumption), while the bound in Abadie et al (2010) is for finite $T\_0$. So that's one question I have -- in practice how should we feel about non-stationary factors?

Moving on to the term $\bar{\zeta}$. I haven’t seen too many people talk about this term, which basically measures the linear independence (correlation) between the factors in the pre-treatment period (lower values indicate higher correlations between factors). I’m not sure why I should have any priors about this value, but its worth noting that if this number is small, this bias bound can also blow up. And also note that not only the linear dependence, but the variance of the factors can affect this term -- the lower variance the factors, the lower we should expect this number. Again thinking of possible COVID-19 applications, we might expect our common factors to be low-variance in the pre-treatment phase, which again can worsen this bound. In general I guess we're just supposed to hope for the best here?

A third thing to notice is that $J$ appears in the numerator -- the more control units we use, the larger this bound. This feels counterintuitive to me – I would expect that the bias would decrease with more control units. Cummins et al (2019) agree with this intuition, noting that theoretically it should be easier to find units to match the unobserved factor loadings with more control units. And when they test this hypothesis, they find that increasing $J$ leads to a modest reduction in bias (Figure 6), which is disappointing to them, but in fact seems to go against what I would expect from this proof. Again I'd be interested to know if anyone has any thoughts on this.

The last part of this bound is a maximum over two terms that both terms decrease with the number of pre-treatment time periods. And the numerators of both of these terms are functions of the scale of the idiosyncratic shocks among the control units; the smaller these shocks, the lower the bound. Cummins et al (2019) have nice illustrations of this effect in their paper. I think this is the most intuitive part of this result, and in general makes sense: the noisier the outcome, the larger the bias, and the more pre-treatment time periods we're able to match on, the lower the bias.

Overall this result is interesting, but if I thought my outcomes were generated by a linear factor model, this bound doesn’t make me feel great about using this method. Especially because we assumed that there was perfect pre-treatment fit and exact balance on the covariates and pre-treatment outcomes. 

Fortunately, Botsaru & Ferman (2019) have a paper on what happens when we don’t exactly balance the covariates – they show that the bias is still bounded under mild conditions. They state:

> the intuition for this result is that it is only possible to have good balance in terms of pre-treatment outcomes for a long set of  pre-treatment periods only if we also have good balance in terms of observed and unobserved covariates.

So maybe we don’t need to worry too much about balancing auxillary covariates as long as we get good balance over the pre-treatment outcomes. This seems to align with the recent literature, which largely side-notes the issue of auxillary covariates.

The worse news comes from Ferman & Pinto (2019), who consider what happens when perfect pre-treatment fit is impossible (which it mostly is). Considering $T\_0 \to \infty$, they show that the estimator is in general biased and inconsistent. The problem is that the objective function will almost always match on noise. As a result, unless the noise is zero or the factors associated with the unbalanced factor loadings are unrelated to treatment assignment, we should never expect to perfectly balance the unobserved factor loadings. While this paper might seem to conflict with the proof in Abadie et al (2010), the authors note that their paper does not assume the existence of weights that exactly balance the pre-treatment outcomes. Yet as they state, the probability of having such a dataset is quite low even with a moderate $T\_0$ unless the variance of $\epsilon\_{it}$ is small.

At the end of the day I'm mostly trying to reconcile different findings I'm seeing in the literature. I realize that they're all dealing with slightly different scenarios, and some are asymptotic results and others are for finite samples. I guess the two things I still feel most unsure about are (1) how we should expect the bias to scale with $J$ in practice; and (2) stationary factors: do these improv or degrade performance? If anyone has any thoughts on this, would be interested to hear them!


