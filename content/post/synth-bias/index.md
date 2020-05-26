---
title: The Linear Factor Model and the Bias of Synthetic Controls
date: 2020-05-24
math: true
diagram: false
---
At least some of the excitement about the Synthetic Controls method comes from the proof in Abadie et al. (2010), where the authors show that assuming the outcome model absent treatment follows a linear factor model, we can bound the bias of the estimator for a finite number of pre-treatment time-periods ($T_0$). A lot of interesting papers have been written this paper analyzing this bound under different assumptions (see, eg, Botosaru & Ferman (2019); Ferman & Pinto (2019)). I’m writing this to summarize some of my thoughts after reading these papers the past several months and to ask a few questions. Let me caveat that I hardly claim to think I'm going to get everything right here -- and if anyone who reads this notices any mistakes, or I’m missing some point, please reach out to me – I’d love to learn more.

The context for the synthetic controls literature is frequently a panel of data with units $j = 0, ..., J$ observed across time-periods $T = 0, ..., T\_0, ..., T$, where unit $j = 0$ is treated at time $T\_0 + 1$. We then posit the following model for the potential outcomes absent treatment:

$$
Y\_{it}^0 = \beta\_tX\_i + \lambda\_t\mu\_i + \epsilon\_{it}
$$

where $\lambda\_t$ is a $(1 x F)$ vector of factors and $\mu\_i$ is an $(F x 1)$ vector of so-called factor loadings. This model is essentially a generalization of the two-way fixed effects model. Specifically, the two-way fixed effects model is the special case with two factors, where $\lambda\_{t, 1} = 1$ for all periods (ie unit fixed-effects) and the second factor has associated factor loadings $\mu\_{i, 1} = 1$ for all units. When thinking about the region-level data typical of these applications, this model basically allows regions to experience common shocks in different ways.

Abadie et al (2010) then prove that the bias of the synthetic controls estimator is bounded in this setting. The proof relies on the existence of weights that exactly balances the means of the control group's covariates $X\_0$ and the pre-treatment outcomes $Y_0$ for $T_0$ time periods to the treated unit's covariate/pre-treatment outcome values. That is, letting $Z = (X, Y)$ we have a $(J x 1)$ vector of weights $w$ such that 

$$
Z\_0'w = Z\_1
$$

That's already a pretty strong condition, especially considering almost no one gets exact mean-balancing weights in any application (more on this later). Nevertheless, we carry on and assume that there are F factors where $\lambda\_{tf}$ are bounded by $\bar{\lambda}$. We also assume that the covariance matrix of these factors in the pre-treatment period $(\frac{1}{T\_0}\sum_{t = 1}^{T\_0}\lambda\_t'\lambda$) is non-singular, and that the minimum eigenvalue is $\bar{\zeta} > 0$. We also let $m\_{p, tj} = \mathbb{E}\mid\epsilon\_{jt}\mid^p$, $m\_{p,j} = \frac{1}{T\_0}\sum\_{t = 1}^{T\_0}m\_{p, tj}$, $\bar{m\_p} = \max\_{j = 1,...,J}m\_{p, j}$ (whew), and lastly we let $\sigma\_{jt} = \mathbb{E}(\epsilon\_{jt}^2)$, $\sigma\_j = \frac{1}{T\_0}\sum\_{t=1}^{T\_0}\sigma\_{jt}^2$, $\bar{\sigma} = \max\_{j = 1, ..., J} \sqrt{\sigma\_j^2}$.

The authors then show that the bias of this estimator is bounded by the following frightening expression:

$$
C\frac{\bar{\lambda}F}{\bar{\zeta}}\max(\frac{\bar{m\_p}^{\frac{1}{p}}}{T\_0^{1 - \frac{1}{p}}}, \frac{\bar{\sigma}}{T\_0^{\frac{1}{2}}})
$$ 

There are a couple of things worth noting here (and these things have been noted before). First of all we assumed that the maximum value of the unobserved factors is bounded across all time periods from $t = 1,..., T$. Imagine trying to run this algorithm on some exponentially increasing process, like, for instance, COVID-19 deaths. You may think that over some fixed time period the value of these factors are bounded, but this result suggests that the bias bound might be pretty bad when we have outcomes that increase exponentially over time (non-stationary outcomes). This suggests to me that analyzing a stationary outcome should be easier. 

On the other hand, I just read Ferman & Pinto (2019), who seem to suggest that asymptotically, matching on factor loadings with non-stationary outcomes is easier. In particular, they show that a demeaned version of the synthetic controls estimator (ie, subtract the pre-treatment mean for each unit) will exactly balance these factor loadings if weights that balance those factor loadings exist! This seems counterintuitive to me given this bound; moreover, Cummins et al (2019) did some nice simulations on synthetic controls. In that paper they fix $T\_0$ and run simulations with different polynomial time trends, and the bias gets worse when adding polynomial factors for p > 1 (Figure 7). I’m not sure how to reconcile this, although Ferman & Pinto's result is asymptotic and relies on demeaning the outcomes (which, importantly, is a different identifying assumption), while the bound in Abadie et al (2010) is for finite $T\_0$.

Let's move on to the term $\bar{\zeta}$. I haven’t seen too many people talk about this term, which basically measures the linear independence (correlation) between the factors in the pre-treatment period (lower values indicate higher correlations between factors). I’m not sure why I should have any priors about this value, but its worth noting that if this number is small, this bias bound can also blow up. And also note that not only the linear dependence, but the variance of the factors can affect this term -- the lower variance the factors, the lower we should expect this number. Again thinking of possible COVID-19 applications, we might expect our common factors to be low-variance in the pre-treatment phase, which again can worsen this bound.

A third thing to notice is that $J$ appears in the numerator -- the more control units we use, the larger this bound. This feels counterintuitive to me – I would expect that the bias would decrease with more control units. Cummins et al (2019) agree with this intuition, noting that theoretically it should be easier to find units to match the unobserved factor loadings with more control units. Moreover, when they test this hypothesis, they find that increasing $J$ leads to a modest reduction in bias (Figure 6). I’m not sure what to make of this result, and would be interested to know if anyone has any thoughts on this.

The last part of this bound is a maximum over two terms that both terms decrease with the number of pre-treatment time periods. And the numerators of both of these terms are functions of the scale of the idiosyncratic shocks among the control units; the smaller these shocks, the lower the bound. Cummins et al (2019) have nice illustrations of this effect in their paper.

Overall I think this result is interesting, but if I thought my outcomes were generated by a linear factor model, this bound doesn’t make me feel great about using this method. Especially because we assumed that there was perfect pre-treatment fit and exact balance on the covariates and pre-treatment outcomes. 

Fortunately, Botsaru & Ferman (2019) have a paper on what happens when we don’t exactly balance the covariates – they show that the bias is still bounded under mild conditions. They state:

> the intuition for this result is that it is only possible to have good balance in terms of pre-treatment outcomes for a long set of  pre-treatment periods only if we also have good balance in terms of observed and unobserved covariates.

So maybe we don’t need to worry too much about balancing auxillary covariates as long as we get good balance over the pre-treatment outcomes. This seems to align with the recent literature, which largely side-notes the issue of auxillary covariates.

The worse news comes from Ferman & Pinto (2019), who consider what happens when perfect pre-treatment fit is impossible (which it mostly is). Considering $T\_0 \to \infty$, they show that the estimator is in general biased and inconsistent. The problem is that the objective function will almost always match on noise. As a result, unless the noise is zero or the factors associated with the unbalanced factor loadings are unrelated to treatment assignment, we should never expect to perfectly balance the unobserved factor loadings. While this paper might seem to conflict with the proof in Abadie et al (2010), the authors note that their paper does not assume the existence of weights that exactly balance the pre-treatment outcomes. Yet as they state, the probability of having such a dataset is quite low even with a moderate $T\_0$ unless the variance of $\epsilon\_{it}$ is small.

And I'm not trying to argue against using synthetic controls in practice. In fact, I find the placebo studies (treatment is backed up, and the procedure re-run to predict observed outcomes) particularly compelling in actual papers. I also like that the idea is simple and intuitive -- there's a lot to be said for explainability. My biggest question pertains to the focus on the linear factor model to justify this procedure -- these theoretic results don't strike me confidence inspiring. So why not instead motivate this procedure from a no unmeasured confounding assumption? We could then do a sensitivity analysis if we wanted to see the robustness of the results against this violation (I still haven't read Firpo & Possebom (2017) which might relate to this point).

That said, thinking about the linear factor model is interesting, and I wonder how we might potentially generalize these results to thinking about what happens when we balance on proxy variables. A lot of the balancing-weights literature for cross-sectional contexts (eg Stable Balancing Weights, CBPS, Entropy Balancing) only considers the case where we measure our covariates correctly. This is a special case where we observe repeated proxies over time for the same underlying signal, and the results show that in general we shouldn't expect to eliminate the bias created by the noise even asymptotically. Can we say something more general about balancing on proxies though? For example, how do these result connect (or not) with Wang & Blei (2019) blessings of multiple causes paper (which considers the case of using mutiple causes as a proxy for underlying confounding)? What's the most general form of the confounding when this procedure might work (that is, relaxing the assumptions of the linear factor model)? A lot of interesting things remain to think about.


