---
layout: single
title:  "A whole lot of not much"
header:
  teaser: "unsplash-gallery-image-2-th.jpg"
categories:
  - Jekyll
tags:
  - research
  - failure
---
This is a story about some work that I spent a lot of time on during my PhD, but that didn't go anywhere. I'm presenting it here as a blog post because I don't think it's suitable for publication in a journal, but is worth being publicly available.

Before I begin I want to make it clear that I take complete ownership of where this research did and did not go.

My project began when I read [this paper](https://www.pnas.org/content/111/49/17546.short) by Vasilis Dakos and Jordi Bascompte, titled "Critical slowing down as early warning for the onset of collapse in mutualistic communities". At the time, I was relatively early in my PhD, and looking for an interesting problem to work on. Broadly speaking, my goal was to use dynamical systems theory to shed light on the mechanism behind some kind of collective behavior. The Dakos \& Bascompte paper seemed like a perfect jumping-off point, because they use a very straighforward piece of theory (linear stability analysis) to study an obviously compelling phenomenon (abrupt extinction events).

To summarize Dakos \& Bascompte's paper in one sentence, they find that in a certain model of population dynamics in a mutualistic ecosystem, extinction events occur as saddle-node bifurcations with respect to certain environmental variables. That immediately implies that the variance and autocorrelation of the state variables (i.e. species abundances) will increase when the system is about to undergo an extinction event, provided that the system is subject to noise. They also find (empirically) that variance and autocorrelation at the species level show some relationship with species extinction risk, and with the network properties of degree (i.e. number of mutualistic partners) and contribution to nestedness.

It was this last part, the relation between network properties and indicators of extinction risk, that I found most intriguing and potentially worth building on. The underlying network - the data of which species interact with which others - can be highly structured. Two notable sorts of structure that have been observed in mutualistic networks specifically are [modularity](https://www.pnas.org/content/104/50/19891) and [nestedness](https://www.pnas.org/content/100/16/9383), though the existence and origin of the latter has been the subject of much debate (see [this recent paper](https://journals.aps.org/prx/abstract/10.1103/PhysRevX.9.031024), for example). And on the dynamical side, there is much more information in a multivariate stochastic process than just the variance of each component; there's a whole covariance matrix!

So, what is there meaningful to say about the relationship between modularity and nestedess on the one hand, and the covariance matrix of the vector of species abundances on the other?
* One might reasonably expect that if the underlying network is modular, then that modular structure should show up in the correlation matrix. That is, species occupying the same module will tend to be more correlated with each other than with species in other modules.
  * As with any application of modularity to real networks, there's the problem of ground truth/detectability
  * There are methods, due to McMahon and Garlaschelli ([here](https://journals.aps.org/prx/abstract/10.1103/PhysRevX.5.021006)) to separate out the spectral components of a correlation matrix that are significantly different to what you'd expect by chance. You can then use that to define a modularity quality function based on the correlation matrix, and hopefully then compare the outcome to what you get from the structural network.
* I looked in various ways at the spectral decomposition of the correlation matrix as the system passes through extinction events. The baseline here is that the leading eigenvalue approaches zero leading up to the first extinction event.
  * I was hopeful that looking at the leading eigenvector would give more information... I did find that often, when multiple species went extinct at once, that their entries in the leading eigenvector of the correlation matrix would be simultaneously large. I don't feel that I found a "smoking gun", but perhaps there's something precise to say here.
  * There is a theoretical reason to think that a spectral decomposition is the right way to look at a covariance matrix, and it traces back to local linearization. If you have a linear stochastic process, then the dynamical matrix, noise matrix, and covariance matrix together satisfy a [Lyapunov Equation](https://en.wikipedia.org/wiki/Lyapunov_equation). Linearizing the population dynamics gives you a dynamical matrix with the same sparsity pattern as the mutualistic network, so you might have some hope of retaining some of the spectral properties (Fiedler vector?).
* I looked at simply the value of correlation between two species' abundances, and plotted how that varied with network distance between the species.
  * I found that correlation decays with network distance, and that this decay is somewhat slower when the system is near a tipping point. Again, no smoking gun.
  * There's the complicating factor that in considering "network distance" I only considered the mutualistic links, whereas there were competitive links in the dynamics among all species of the same guild, meaning that effectively the diameter of the network was 2.
* I looked at randomizing the links in the network and seeing how certain dynamical properties changed, such as the distribution of extinction times. I found a very mild signal that extinction events occur closer to each other in networks with higher nestedness, but nothing I considered worth investigating further.
* One of the main confounding issues I found was the enormity of the parameter space of the model I was working with. For starters, each species has an intrinsic growth rate, assigned at random. I tried imposing the strongly simplifying that all growth rates are equal, and actually obtained a branch of solutions that meets the zero state at a saddle-node bifurcation. If there is zero competition, then this branch is stable down to the bifurcation and it reflects what you see in simulation. But if you add competition, the branch destabilizes before it meets the zero branch! In simulations, that destabilization point coincides with the first extinction event, and is preceded by critical slowing down. Unfortunately I haven't found a clean description of the unstable manifold or the other state the system eventually finds.

Overall, I learned several interesting facts about a particular model class derived from ecology. But what I don't see here is any cohesive "lesson learned" about multivariate early warning signs for tipping points in ecological systems. To put it bluntly, I learned a whole lot of not much.

So what happened?

My takeaway is that this project was never properly scoped. I wasn't committed to discovering any particular ecological fact, but I kept myself constrained to an ecologically-derived model class because I wanted to be in a context where particular network properties (modularity and nestedness) were present and presumably important.

More broadly, this work didn't start with a specific question. I had some ideas of the form: X should be related to Y in some interesting way. I did a whole mess of simulation and data analysis in hopes that I would find an answer to *some* question, then figure out the question it was the answer to, and that would be my paper. In the end I did end up
