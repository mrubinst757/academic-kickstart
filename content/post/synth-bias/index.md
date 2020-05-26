---
title: The Linear Factor Model and Synthetic Controls
date: 2020-05-24
math: true
diagram: false
---
At least some of the excitement about the Synthetic Controls method comes from the proof in Abadie et al. (2010), where the authors show that assuming the outcome model absent treatment follows a linear factor model, we can bound the bias of the estimator for a finite number of pre-treatment time-periods ($T_0$). A lot of interesting papers have been written this paper analyzing this bound under different assumptions (see, eg, Botosaru & Ferman (2019); Ferman & Pinto (2019)). I’m writing this to summarize some of my thoughts after reading these papers the past several months and to ask a few questions. Let me caveat that I hardly claim to think I'm going to get everything right here -- and if anyone who reads this notices any mistakes, or I’m missing some point, please reach out to me – I’d love to learn more.

The context for the synthetic controls literature is frequently a panel of data with units $j = 0, ..., J$ observed across time-periods $T = 0, ..., T\_0, ..., T$, where unit $j = 0$ is treated at time $T\_0 + 1$. We then posit the following model for the potential outcomes absent treatment:

$$
Y\_{it}^0 = \beta\_tX\_i + \lambda\_t\mu\_i + \epsilon\_{it}
$$

where $\lambda\_t$ is a $(1 x F)$ vector of factors and $\mu\_i$ is an $(F x 1)$ vector of so-called factor loadings. This model is essentially a generalization of the two-way fixed effects model. Specifically, the two-way fixed effects model is the special case with two factors, where $\lambda\_{t, 1} = 1$ for all periods (ie unit fixed-effects) and the second factor has associated factor loadings $\mu\_{i, 1} = 1$ for all units. When thinking about the region-level data typical of these applications, this model basically allows regions to experience common shocks in different ways, and explains the residual cross-sectional and time-series dependence in the data if we only controlled for covariates $X\_i$.

Abadie et al (2010) then prove that the bias of the synthetic controls estimator is bounded in this setting (I'm not going to review this estimation strategy, but I encourage you to read the paper if you haven't read it!). The proof relies on the existence of weights that exactly balances the means of the control group's covariates $X\_0$ and the pre-treatment outcomes $Y_0$ for $T_0$ time periods to the treated unit's covariate/pre-treatment outcome values. That is, letting $Z = (X, Y)$ we have a $(J x 1)$ vector of weights $w$ such that 

$$
Z\_0'w = Z\_1
$$

That's already a pretty strong condition, especially considering almost no one gets exact mean-balancing weights in any application (more on this later). Nevertheless, we carry on and assume that there are F factors where $\lambda\_{tf}$ are bounded by $\bar{\lambda}$. We also assume that the covariance matrix of these factors in the pre-treatment period $(\frac{1}{T\_0}\sum_{t = 1}^{T\_0}\lambda\_t'\lambda$) is non-singular, and that the minimum eigenvalue is $\bar{\zeta} > 0$. We also let $m\_{p, tj} = \mathbb{E}\mid\epsilon\_{jt}\mid^p$, $m\_{p,j} = \frac{1}{T\_0}\sum\_{t = 1}^{T\_0}m\_{p, tj}$, $\bar{m\_p} = \max\_{j = 1,...,J}m\_{p, j}$ (whew), and lastly we let $\sigma\_{jt} = \mathbb{E}(\epsilon\_{jt}^2)$, $\sigma\_j = \frac{1}{T\_0}\sum\_{t=1}^{T\_0}\sigma\_{jt}^2$, $\bar{\sigma} = \max\_{j = 1, ..., J} \sqrt{\sigma\_j^2}$.

The authors then show that the bias of this estimator is bounded by the following frightening expression:

$$
C\frac{\bar{\lambda}F}{\bar{\zeta}}\max(\frac{\bar{m\_p}^{\frac{1}{p}}}{T\_0^{1 - \frac{1}{p}}}, \frac{\bar{\sigma}}{T\_0^{\frac{1}{2}}})
$$ 

There are a couple of things worth noting here (and I’m hardly the first one to note them). First of all we assumed that the maximum value of the unobserved factors is bounded across all time periods from $t = 1,..., T$. Imagine trying to run this algorithm on some exponentially increasing process, like, for instance, COVID-19 deaths. You may think that over some fixed time period the value of these factors are bounded, but this result suggests that the bias bound might be pretty bad when we have outcomes that increase exponentially over time (non-stationary outcomes). This suggests to me that analyzing a stationary outcome should be easier. 

On the other hand, I just read Ferman & Pinto (2019), who seem to suggest that asymptotically, matching on factor loadings with non-stationary outcomes is easier. In particular, they show that a demeaned version of the synthetic controls estimator (ie, subtract the pre-treatment mean for each unit) will exactly balance these factor loadings if weights that balance those factor loadings exist! This seems counterintuitive to me given this bound; moreover, Cummins et al (2019) did some nice simulations on synthetic controls. In that paper they fix $T\_0$ and run simulations with different polynomial time trends, and the bias gets worse when adding polynomial factors for p > 1 (Figure 7). I’m not sure how to reconcile this, although Ferman & Pinto's result is asymptotic and relies on demeaning the outcomes (which, importantly, is a different identifying assumption), while the bound in Abadie et al (2010) is for finite $T\_0$.

Let's move on to the term $\bar{\zeta}$. I haven’t seen too many people have qualms about this term, which basically measures the linear independence (correlation) between the factors (lower values indicate higher correlations between factors). I’m not sure why I should have any priors about this value, but its worth noting that if this number is small, this bias bound can also blow up. Not only can linear dependence between the factors drive down this value, but so can the variance of the factors. In particular, the lower variance the factors, the lower we should expect this number. Again thinking of possible COVID-19 applications, we might expect our common factors to be low-variance in the pre-treatment phase, which again can worsen this bound.

A third thing to notice is that $J$ appears in the numerator -- the more control units we use, the larger this bound. This feels counterintuitive to me – I would expect that the bias would decrease with more control units. Cummins et al (2019) agree with this intuition, noting that theoretically it should be easier to find units to match the unobserved factor loadings with more control units. Moreover, when they test this hypothesis, they find that increasing $J$ leads to a modest reduction in bias (Figure 6). I’m not sure what to make of this result.

The last part of this bound is a maximum over two terms that both terms decrease with the square root of the number of pre-treatment time periods. Excellent! And the numerators of both of these terms are functions of the scale of the idiosyncratic shocks among the control units; the smaller these shocks, the lower the bias. Cummins et al (2019) have nice illustrations of this effect in their paper.

Overall I think this result is interesting, but if I thought my outcomes were generated by a linear factor model, this bound doesn’t make me feel great about using this method. Especially because we assumed that there was perfect pre-treatment fit and exact balance on the covariates and pre-treatment outcomes. 

Fortunately, Botsaru & Ferman (2019) have a paper on what happens when we don’t exactly balance the covariates – they show that the bias is still bounded under mild conditions. They state:

> the intuition for this result is that it is only possible to have good balance in terms of pre-treatment outcomes for a long set of  pre-treatment periods only if we also have good balance in terms of observed and unobserved covariates.

So maybe we don’t need to worry too much about balancing auxillary covariates as long as we get good balance over the pre-treatment outcomes.

The worse news comes from Ferman & Pinto (2019), who consider what happens when perfect pre-treatment fit is impossible (which it mostly is). Considering $T\_0 \to \infty$, they show that the estimator is in general biased and inconsistent. The problem is that the objective function can essentially match on noise. As a result, unless the noise is zero or the factors associated with the unbalanced factor loadings are unrelated to treatment assignment, we should never expect to perfectly balance the unobserved factor loadings. While this paper might seem to conflict with the proof in Abadie et al (2010), the authors note that their paper does not assume the existence of weights that exactly balance the pre-treatment outcomes. As they state, the probability of having such a dataset is quite low even with a moderate $T\_0$ unless the variance of $\epsilon\_{it}$ is small.

At the end of the day I don’t want to argue against using synthetic controls in practice. It’s a nice, intuitive idea and I think there is a lot to be said for methods that you can actually explain to people. My biggest questions pertain to the focus on the linear factor model to justify this procedure -- why not instead assume no unmeasured confounding? We could then do a sensitivity analysis if we wanted to see the robustness of the results against this violation.

On the other hand, I will say that one of the fun reasons to think about the linear factor model is because of its generalizability to thinking about what happens when we balance on noisy covariates more generally. A lot of the balancing-weights literature for cross-sectional contexts (eg SBW, CBPS, Entropy Balancing) only considers the case where we measure our covariates correctly. This is a special case where we observe repeated proxies for the same underlying noise, and the literature shows that in general we can’t get rid of this bias. That’s worth knowing! And there are a lot of aspects of this problem that could potentially be generalized (I also want to think through more how this result connects -- or doesn't -- with Wang & Blei (2019) blessings of multiple causes paper). Even so, in practice sensitivity analyses under the no unmeasured confounding assumption seem like a good and underused idea in this setting (see, eg, Rudolph & Stuart (2018)).
