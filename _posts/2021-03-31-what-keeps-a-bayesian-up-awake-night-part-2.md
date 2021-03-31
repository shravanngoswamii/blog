---
layout:      post
title:       "What Keeps a Bayesian Up At Night? Part 2: Night Time"
tags:        [theory, foundations]
authors:
    - name: Wessel Bruinsma
      link: https://wesselb.github.io
    - name: Richard E. Turner
      link: http://cbl.eng.cam.ac.uk/Public/Turner/Turner
comments:    true
image:      /assets/images/what-keeps-a-bayesian-awake-at-night/night.jpg
image_attribution:
    name: Guy Beauchamp
    link: https://www.flickr.com/photos/mfatic/38634125441/in/photolist-21RXZv4-ZvujyT-27fNfFQ-bbDGs8-okpqen-9g1DDQ-dE4Vqz-tcKsYF-SViHjU-UeQsbU-bGfrha-j7aDEf-9Aj9JH-9ZHmr9-9rQ9fM-21Fqptx-dEaktE-9WVFdr-23j64jn-kh6Rau-kJtYqw-oxHDzs-dP3T6H-dE4VUZ-MosJki-Hi36W3-9kTT18-9gSkux-psHoPD-9srVkL-pQVgKq-b4qMtZ-8TKTjk-22vNHba-a3Cor1-us7ouW-biryiV-9sjyCo-23i9cXG-auLhmQ-2742AHY-bBKxsK-bkFd2y-pdPyCj-9s1TFJ-dEaeSm-dEafoQ-qiB5DS-98QYCY-axGtGo
excerpt: |
    <i>The theory of subjective probability describes ideally consistent behaviour and ought not, therefore, be taken too literally. <br>
    — Leonard Jimmie Savage (1917–1971)</i>
---

> *The theory of subjective probability describes ideally consistent behaviour and ought not, therefore, be taken too literally.*
> 
> --- *Leonard Jimmie Savage (1917--1971)*


In the [first post]({% post_url 2021-03-31-what-keeps-a-bayesian-awake-at-night-part-1 %}) in this series, we laid out the standard arguments that we and many others have used to support the edifice which is Bayesian inference. In this post, we identify the weaknesses in these arguments that cause us to lose sleep at night.

We've split these weaknesses into three types: first, weaknesses in the standard mathematical justifications for the probabilistic approach; second, weaknesses arising in practice from the modelling stage; and, third, weaknesses arising from realising the inference stage due to computational constraints.

## Weakness 1: Standard justifications have problems

We have seen that probability theory and Bayesian decision theory are usually justified in one of several ways, but do these standard justifications *really* stand up to scrutiny? Let's go through each of the seven arguments in turn.

**(1) de Finetti's exchangeability theorem** presupposes a probability distribution over the data and shows this naturally leads to distributions over parameters. However, it does not justify the use of probability in the first place.

**(2) Cox's theorem** does justify the probabilistic approach, but making the argument watertight turns out to be far more delicate than the textbooks, say of [Jaynes (2003)](https://bayes.wustl.edu/etj/prob/book.pdf) or [Bishop (2006)](https://www.microsoft.com/en-us/research/uploads/prod/2006/01/Bishop-Pattern-Recognition-and-Machine-Learning-2006.pdf), would have you believe. To make the theorem mathematically rigorous requires additional technical assumptions that muddy the clarity of the argument. [Paris (1994, p. 24)](https://doi.org/10.1017/CBO9780511526596): "[W]hen an attempt is made to fill in all the details some of the attractiveness of the original is lost."[^1] Moreover, there remains disagreement about the desirability of several of Cox's theorem's other assumptions.[^2] Perhaps the most controversial assumption is that plausibilities are represented by real numbers. As a consequence, for *every two possible propositions*, one of the two propositions conclusively has higher (or equal) belief. (This is called *universal comparability*.) But what if we are truly ignorant about two matters, or have not yet formed an opinion? In that case, is it reasonable to require that the plausibilities assigned to the matters are necessarily comparable?[^3]

**(3) The Dutch book argument** tells us that any coherent actor should use probability to express their beliefs, but the argument leaves the door open as to how the actor should update their beliefs in light of new evidence ([Hacking, 1976](https://doi.org/10.1086/288169)). This is because the standard Dutch book setup is *static*: it does not involve a step where beliefs are updated on the basis of new information. That Bayes' rule should be used for this purpose requires additional assumptions. *Dynamic* alternatives of the Dutch book argument attempt to fix this flaw, but again the force of the argument is diminished and open to criticism ([Skyrms, 1987](https://doi.org/10.1086/289350)).[^4]

**(4) Savage's theorem** guarantees a nearly unique loss function (unique up to an affine transform) and a unique probability, which together form the Bayes risk. The troubling aspect of Savage's theorem is that the constructed loss function depends *only* on the outcome of a decision, and Savage's axioms imply that outcomes of decisions have a value which is independent of the state of the world ([Karni, 2005](http://www.econ2.jhu.edu/people/Karni/savageseu.pdf)). As [Wakker & Zank (1998)](https://doi.org/10.1287/moor.24.1.8) remark, this disentanglement can be undesirable. They give the example of health insurance, where the value of money depends on sickness or health. If the loss function is allowed to depend on other aspects of the world, then the probability constructed by the theorem is no longer unique ([Karni, 2005](http://www.econ2.jhu.edu/people/Karni/savageseu.pdf)). For this reason, [Karni (2005)](http://www.econ2.jhu.edu/people/Karni/savageseu.pdf) argues that the probability constructed by the theorem is arbitrary and thus cannot realistically represent the decision maker's beliefs.

**(5) Doob's consistency theorem**, the **(6) optimality of Bayesian predictions** and **(7) Wald's theorem** are all only unit tests: how reassured should we really be that Bayesian inference passes them? It is not clear that the guarantees on the optimality of Bayesian predictions are the guarantees you care about. For example, typically we're faced with a single dataset and care only about performance on it alone, rather than the average performance across many potential datasets. Similarly, the admissibility of a decision rule in Wald's theorem is desirable but not sufficient: indeed, there are admissible estimators that are not reasonable.[^5] Doob's consistency theorem also suffers from subtle theoretical issues ([Diaconis & Freedman, 1986](https://doi.org/10.1214/aos/1176349830)), which we discuss below in the context of model mismatch.

**Take away.** It is clear then, that uniquely and precisely justifying the three stage Bayesian approach via a single argument is much more delicate than many would have you believe. However, these issues don't trouble our sleep. That is partly due to the fact that it is reassuring that so many and so diverse a set of arguments suggest that it is a sensible approach.[^6] However, it is also because there are far bigger issues to worry about.

## Weakness 2: Modelling is hard and inferences are sensitive to innocuous details

All practitioners of Bayesian inference will know well that beliefs are typically only roughly encoded into probabilistic models. There are at least three good reasons for this. First, it is often hard to really pin down precisely what you believe: What tail behaviour should a variable have? Are there latent variables at play? *Et cetera.* Second, even when you do have an ideal model in mind, it can be mathematically challenging to accurately translate that into a probability distribution. Third, many modelling choices are often based on convenience such as mathematical tractability.  

Should we be worried about this? Surely roughly encoding our prior knowledge is sufficient? 

Unfortunately, seemingly small or irrelevant inaccuracies in the model can greatly affect the posterior and therefore downstream decision making. For example, [Diaconis & Freedman (1986)](https://doi.org/10.1214/aos/1176349830) show that "in high-dimensional problems, arbitrary details of the prior really matter". Similarly, [Kass & Raftery (1995)](https://doi.org/10.2307/2291091) conclude that, in the context of Bayesian model comparison, "[t]he chief limits of Bayes factors are their sensitivity to the assumptions in the parametric model and the choice of priors." Indeed, for this reason, the textbook presentation of model comparison, such as that in [David MacKay's excellent text book](http://www.inference.org.uk/mackay/itila/), should be seen as only a pedagogical depiction of Bayesian inference at work, rather than an approach that will bear practical fruit: discrete Bayesian model comparison does not work in practice.[^7] As a more recent example of the importance of priors in high-dimensional settings, experiments by [Wenzel *et al.* (2020)](https://arxiv.org/abs/2002.02405) suggest that the usual choice of a Gaussian prior for Bayesian neural network models contributes to the *cold posterior effect* in which the Bayesian posterior is outperformed on prediction tasks by strongly tempered versions. 

Can theory about the performance of the Bayesian approach in the face of model misspecification act as a comfort blanket? There are theorems, analogous to Doob's consistency theorem in the well-specified case, that describe situations when the Bayesian posterior will still concentrate on the true parameter value even when the model is misspecified ([Kleijn & Van der Vaart, 2006](https://arxiv.org/abs/math/0607023); [De Blasi & Walker, 2013](https://www.jstor.org/stable/24310519); [Ramamoorthi *et al.*, 2015](https://arxiv.org/abs/1312.4620)), but, as [Grünwald & van Ommen (2017)](https://arxiv.org/abs/1412.3730) point out[^8], "[these theorems hold] under regularity conditions that are substantially stronger than those needed for consistency when the model is correct."[^9]

Theoretical understanding of the consequences of model misspecification is not yet available to rescue us. The probabilistic approach to modelling therefore poses an unsettling dichotomy: on the one hand, you're free to choose the prior, because it is *your belief*, *your prior*; but on the other hand, you should choose your prior absolutely right, because seemingly small or irrelevant changes can greatly affect the conclusions. 

**A way forward?** This weakness of the Bayesian approach used to keep us awake at night. However, taking a slightly different perspective cures our insomnia. The conventional view dogmatically insists that Bayesians should initially and immutably encapsulate prior assumptions up front in the modelling step before seeing data. They then perform inference and then decision making when the data arrives and at that point they're done. The alternative perspective instead uses the three stage process as a *consistency checking device*: if I made these assumptions, then these would be the corresponding coherent statistical inferences. In this way, we are free to explore a range of different assumptions, assessing their consequences both for the model and inferences we make. We are free to modify the model accordingly until we arrive at something that well approximates what we believe. This view has been called the *hypothetico--deductive* view of Bayesian inference ([Gelman & Shalizi, 2011](https://arxiv.org/abs/1006.3868)) and it has the advantage that this is the way many of us use these methods in practice. The disadvantage is that double-dipping is possible, which requires that you are careful about the checks and diagnostics that you perform on the model and corresponding inferences.

## Weakness 3: Approximate inference

The weakness that kept Zoubin Ghahramani (and many other Bayesians) awake at night is that the Bayesian posterior is computationally intractable in all but the simplest cases. Consequently, approximations are necessary, which means that all the theoretical guarantees and justifications that are true for the exact Bayesian posterior no longer hold. Worse still, we do not know in what ways approximation will affect different aspects of the solution. 

For example, one common approximation technique is *variational inference* ([Wainwright & Jordan, 2008](http://dx.doi.org/10.1561/2200000001)), which side-steps intractable computations arising from application of the sum and product rules by projecting distributions onto simpler tractable alternatives. Variational inference is therefore **guaranteed to be incoherent**: it returns different solutions from exactly applying the sum and product rules. How do these approximate inferences differ from the true ones? 

Variational inference is known to (1) underestimate posterior uncertainty if factorised approximations are used[^10] and (2) bias parameter learning so that overly simple models are returned that underfit the data ([Turner & Sahani, 2011](http://www.gatsby.ucl.ac.uk/~turner/Publications/turner-and-sahani-2011a.html)). An example of the first phenomenon is the observation that the mean-field variational approximation in neural networks is unable to model in-between uncertainty ([Foong *et al.*, 2019](https://arxiv.org/abs/1906.11537)). Examples of the second phenomenon are over-pruning in mixture models ([MacKay, 2001](http://www.inference.org.uk/mackay/minima.pdf); [Blei & Jordan, 2006](https://doi.org/10.1214/06-BA104)), variational autoencoders[^11] ([Burda *et al.*, 2015](https://arxiv.org/abs/1509.00519); [Chen *et al.*, 2016](https://arxiv.org/abs/1611.02731); [Zhao *et al.*, 2017](https://arxiv.org/abs/1706.02262); [Yeung *et al.*, 2017](https://arxiv.org/abs/1706.03643)), and Bayesian neural networks ([Trippe & Turner, 2018](https://arxiv.org/abs/1801.06230)). 

Inevitably, these errors are amplified if repeated approximation steps are required. For example, in *online* or *continual learning* the goal is to incorporate new observations sequentially without forgetting old ones ([Nguyen *et al.*, 2017](https://arxiv.org/abs/1710.10628)). In theory, repeated application of Bayes' rule provides the optimal solution, but in practice approximations like variational inference are required at each step. This causes amplification of the approximation error, which eventually leads the model to "forget" old data. Consequently, Bayesian methods are approximate and have to be checked and benchmarked for catastrophic forgetting, just like their non-Bayesian counterparts. 

We lack a general theory that justifies variational inference. Recent work has shown that it is frequentist consistent ([Wang & Blei, 2017](https://arxiv.org/abs/1705.03439)) under assumptions similar to those required for consistency of Bayesian inference under model misspecification. Although useful, this is a long way short of a general compelling justification for the approach.[^12]

Similar arguments can be made against other approaches to approximate inference such as Monte Carlo methods. For example, *Markov chain Monte Carlo* (MCMC) is guaranteed to eventually give you a close-enough answer; but it may take an unfeasibly long time to do so and diagnosing whether you have reached that point is very difficult (we often rely on heuristics to check whether the chain has been run for long enough). So, although excellent software for performing MCMC exists,[^13] ensuring that it is performing correctly is delicate.[^14]

Let us take a step back and consider approximate inference in the context of Bayesian decision theory. In its ideal form, Bayesian decision theory is a cleanly separated three-step procedure consisting of model specification, inference and loss minimisation. However, in practice the use of approximate inference entangles the three steps: models which are simpler result in more accurate, and therefore more coherent, inference; and downstream decision making dictates what aspects of the true posterior your approximate posterior should capture and therefore feeds into the inference stage rather than being decoupled from it ([Lacoste-Julien *et al.*, 2011](http://proceedings.mlr.press/v15/lacoste_julien11a.html)).[^15]

**A way forward?** To our minds, the jury is still out here about the best way forward. Hopefully a more general theory --- potentially based on decision making under uncertainty and computational constraints that extends Cox's, Ramsey's and de Finetti's, or Savage’s ideas --- will emerge. In the meantime, continuing to provide more limited theoretical characterisation of the properties of existing inference approaches is vital. Practitioners should also test the hell out of their inference schemes to gain confidence in them. Testing on special cases where ground truth posteriors or underlying variables are known is helpful. The acid test is whether your inference scheme works on the real world data you care about, so test cases also need to replicate aspects of this situation. Here the ideas of probabilistic models and being able to sample fake data, potentially from models trained on real data, is very useful. This fits nicely with the hypothetico–deductive loop: specify your model, perform approximate inference, check your inferences, now check your model, review your modelling choices, and start the process again. 

Of course the amazing and diverse practical successes of Bayesian inference --- from [cracking Enigma](http://mathcenter.oxford.emory.edu/site/math117/bayesTheorem/enigma_and_bayes_theorem.pdf) and [finding Air France Flight 447](https://projecteuclid.org/journals/statistical-science/volume-29/issue-1/Search-for-the-Wreckage-of-Air-France-Flight-AF-447/10.1214/13-STS420.full), to [the TrueSkill match-making system](https://en.wikipedia.org/wiki/TrueSkill) --- give us great confidence in the utility of the probabilistic approach.

## Conclusion

We started the first of these two posts by remarking that it was striking that Michael Jordan, Geoff Hinton, and Zoubin Ghahramani --- individuals who had made huge contributions to the probabilistic brand of machine learning --- maintained reservations about that very approach. We hope that this post has brought colour to these concerns.

Michael Jordan thought that Bayesians and frequentists should reconcile and operate arm-in-arm. This makes sense since Bayesians develop inference procedures and frequentist methods can be used to evaluate them.

Geoff Hinton’s view was that Bayesian approaches don’t cut it for the problems he is interested in, like object recognition and machine translation. This is understandable since we have seen that Bayesian methods are useful when (1) you’re able to encode knowledge carefully into a detailed probabilistic model; (2) you are uncertain and want to propagate uncertainty accurately; and (3) you can perform accurate approximate inference. Is this really true in situations like object recognition or machine translation? These problems involve learning complex high-dimensional representations from stacks of largely noise-free images, words, or speech, and, to quote Zoubin Ghahramani, "if our goal is to learn a representation (...) then Bayesian inference gets in the way".[^16]

Zoubin Ghahramani admitted that approximate inference kept him awake at night. We have seen that the arguments that we make to justify the Bayesian approach --- Cox’s and Savage’s theorems, Dutch books, *et cetera* --- are practically meaningless because our inference is approximate. The elegant separation of modelling, inference, and decision making steps, is in fact a tangled web.  

So, when we teach the ideas of probabilistic modelling and inference and write boilerplate in our papers, we’d ask that you pause for breath before trotting off the standard justifications. Instead, consider evangelising a little less about how principled the approach is, mention the importance of exploring different modelling assumptions and their consequences, and stress that the quality of approximate inference should be evaluated in a systematic and principled way.


[^1]: Also quoted by [Terenin & Draper (2015)](https://arxiv.org/abs/1507.06597). [Halpern (1999)](https://arxiv.org/abs/1105.5450) constructs a counterexample to Cox's theorem if the additional technical assumption, [Paris' (1994)](https://doi.org/10.1017/CBO9780511526596) density assumption, is omitted.

[^2]: [Van Horn (2003)](https://doi.org/10.1016/S0888-613X(03)00051-3) concludes that "[they] cannot make a compelling case for all of [Cox's theorem's] requirements, however, and there remains disagreement as to the desirability of several of them."

[^3]: Interestingly, there are two-dimensional theories of probability; see Section 4.1.2 by [Bakker (2016)](https://scripties.uba.uva.nl/download?fid=639254) for an overview.

[^4]: [Skyrms (1987, p. 4)](https://doi.org/10.1086/289350) states a dynamic Dutch book argument due to David Lewis (reported by Paul Teller; 1973, 1976). Skyrms also says (p. 19) that "(...) not every learning situation is of the kind to which conditionalization applies. The situation may not be of the kind that can be correctly described or even usefully approximated as the attainment of certainty by the agent in some proposition in his degree of belief space. The rule of belief change by probability kinematics on a partition was designed to apply to a much broader class of learning situations than the rule of conditionalization." In the paper, Skyrms describes the Observation Game, a learning situation where conditionalisation does not apply, where a generalisation of conditionalisation called *probability kinematic* ([Jeffrey, 1965](https://press.uchicago.edu/ucp/books/book/chicago/L/bo3640589.html)) --- essentially, Bayes' rule with a different prior --- is necessary and, under certain conditions, sufficient to be *bulletproof* --- a strong coherence condition that excludes a Dutch book.

[^5]: For example, see Example 5.7.2 by [Lehmann & Casella](https://doi.org/10.1007/b98854) ([1998](https://doi.org/10.1007/b98854); also [Makani, 1997](https://doi.org/10.1214/aos/1176343853)).

[^6]: [Savage's (1962)](https://doi.org/10.2307/2281641) comment on personal probability that was reproduced at the start of this post captures this sentiment well. 

[^7]: Andrew Gelman and David MacKay discuss this issue [here](https://statmodeling.stat.columbia.edu/2011/12/04/david-mackay-and-occams-razor/).

[^8]: See also [this great answer](https://stats.stackexchange.com/a/279798) by Peter Grünwald on StackExchange.

[^9]: As a solution, [Grünwald & van Ommen (2017)](https://arxiv.org/abs/1412.3730) propose to replace the posterior by a generalised posterior, which depends on a learning rate. This, however, requires an appropriate choice of the learning rate, obscures the meaning of the likelihood, and, more importantly, invalidates the fundamental justifications which hold for Bayes' rule. 

[^10]: Often the approximations involve some form of factorisation assumption, but alternatives exist that have different properties. For example, inducing point approximations for Gaussian processes ([Titsias, 2009](http://proceedings.mlr.press/v5/titsias09a.html)) tend to overestimate uncertainty.

[^11]:  As an attempt to improve the variational approximation, it has been proposed to recalibrate the variational objective by reweighting the KL term ([Alemi *et al.*, 2017](https://arxiv.org/abs/1612.00410); [Higgings *et al.*, 2017](https://openreview.net/forum?id=Sy2fzU9gl)), which is closely related to the cold posterior effect. But by attempting to fix variational inference in this way, we are blurring the lines between Bayesian modelling and end-to-end optimisation, producing cleverly regularised estimators rather than models which reason according to fundamental principles.

[^12]: A pragmatic solution identifies standard practice, which uses point estimates for the unknowns such as $\hat X_{\text{MAP}}$, as effectively summarising the posterior $p(X \cond D)$ by a Dirac delta function, a probability distribution concentrated on $\hat X_{\text{MAP}}$. [David MacKay (2003, Section 33.6)](http://www.inference.org.uk/mackay/itila/) say that, "[f]rom this perspective, *any* approximating distribution $Q(x;\theta)$, no matter how crummy it is, *has* to be an improvement on the spike produced by the standard method!" However, "has" is doing a lot of work in this sentence.

[^13]: For example, [Stan](https://mc-stan.org/), [PyMC3](https://docs.pymc.io/), or [Turing.jl](https://turing.ml/), but there is [more out there](https://en.wikipedia.org/wiki/Probabilistic_programming#List_of_probabilistic_programming_languages).

[^14]: Micheal Betancourt has a [great post](https://betanalpha.github.io/assets/case_studies/markov_chain_monte_carlo.html#5_robust_application_of_markov_chain_monte_carlo_in_practice) about responsible use of MCMC in practice.

[^15]: Indeed, the idea of decision-making-aware inference has been successful in the meta-learning setting ([Gordon *et al.*, 2019](https://arxiv.org/abs/1805.09921)). It seems likely that the entanglement of inference and decision making (as a consequence of the intractability of the posterior) could justify the recent trend towards end-to-end systems, which directly optimise for the metric of interest, thereby circumventing these issues.

[^16]: The development of "Bayesian-inspired" approaches, which blend end-to-end deep learning with ideas from probabilistic modelling and inference is arguably a reaction to these concerns. The goal of these developments is to provide excellent representation learning and strong predictive performance whilst still handling uncertainty, but without claiming strict adherence to the Bayesian dogma. 