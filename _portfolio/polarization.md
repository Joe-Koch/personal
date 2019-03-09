---
layout: single
title: "Polarization"
excerpt: "A simulation of polarization in the scientific community."
header:
  image: /assets/images/polarization/large25.gif
author_profile: true
sidebar:
  - text: "<a href='https://github.com/ZoeKoch/polarization/blob/master/fastpolarize.m'>View the project's code</a>"
toc: true
tags:
  - simulation
  - computational modeling
  - MATLAB
---

<script type="text/x-mathjax-config">
  MathJax.Hub.Config({
    tex2jax: {
      inlineMath: [ ['$','$'], ["\\(","\\)"] ],
      processEscapes: true
    }
  });
</script>
<script src='https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.5/MathJax.js?config=TeX-MML-AM_CHTML' async></script>

# Simulating Ideological Polarization

## Introduction

How does ideological polarization, where subgroups within a society maintain stable opposing beliefs, arise? This agent-based simulation demonstrates a mechanism by which a group of learners who share evidence, and who have the same aims and values, can nonetheless become polarized. The model is based on the framework proposed in Scientific Polarization[^1], and explores how the altering the size of one's cohort affects rates of polarization and convergence on the correct belief. 

## Implementation 

**tl;dr:** Each scientist is trying to determine which action is better, A (an acceptable choice) or B (a better choice). If they believe action B is better, they’ll create more evidence about action B. They’ll look at the evidence of their nearest neighbors, as well, but will believe that the evidence is false if they deem their neighbor an untrustworthy source. Trustworthiness is partially based on how similar their beliefs were to begin with. They then update their beliefs based on the new evidence and their perceived probability that the evidence is true. The simulation continues until the beliefs are stable, ending either in community consensus or polarization.
{: .notice}

We start with the community of scientists arranged in a square grid, where each agent in the community has some uniformly distributed random “credence” between 0 and 1 that action B is better than action A. The success rate of action A is known to be .5, while that of action B is unknown to the scientists. We choose some probability of success for action B, called $$P_B$$, between .5001 and .8. At each timestep, each scientist who believes action B is better than action A performs an experiment where they try action B $$n$$ times and count the successes (a Bernoulli trial).

The scientists also each have an unchanging neighborhood of size $$degree$$ comprised of the scientists nearest them. Each scientist then updates their credence that B is better than A based on the new evidence of any scientists in their neighborhood. This update is modeled using the equation given in the original paper, and uses a combination of Bayes’ rule, Jeffrey conditionalization, and the premise that scientists may disbelieve in the experimental results of scientists they perceive to be untrustworthy sources. 

<p> <b>The updates, in mathematical detail:</b> 
  <br> Suppose Jill is looking at Ian’s evidence and determining how it should affect her beliefs that action B is better than action A. Jill might not fully trust Ian’s credibility, meaning she has credence $P_f(E) \leq 1$ that his evidence is false. Under Jeffrey conditionalization Jill will update her beliefs, in light of Ian’s evidence, using the following formula: $$P_f(H) = P_i(H|E) \cdot P_f (E) + P_i(H \vert ~ E) \cdot P_f (~ E)$$ This says that Jill's final belief in the hypothesis that B is better than A is equal to her credence that the evidence is real, $P_f (E)$, multiplied by the belief she would obtain via strict conditionalization on that evidence, $P_i(H | E)$, plus her credence that the evidence did not occur, $P_f (~ E)$, multiplied by the belief she would have by strict conditionalization if it had not occurred, $P_i(H|~ E)$. Here, the strict conditionalization updates are given by Bayes' rule: 
  $$P_i(H|E) = \frac{P_i(E|H) \cdot P_i(H)}{P(E)}$$ 
  The formula for how much Jill trusts Ian's evidence is given by 
  $$P_f (E) =  \max(1 - d \cdot m \cdot (1 - P_i(E), 0)$$ 
  Where $d$ is the distance between Jill's and Ian's credences in the hypothesis, $m$ is a multiplier that captures how quickly agents become uncertain about the evidence of their peers as their beliefs diverge, and $P_i(E)$ is Jill’s initial probability of the evidence occurring given her credence in the hypothesis. This formula, as described in the original paper, is fairly arbitrary. The important thing is that as the distance between Jill's and Ian's beliefs grows, Jill finds Ian's evidence less credible. She then may update her beliefs away from what Ian's evidence suggests.
</p>
{: .notice}

Once the scientists’ credences are stable or the pre-set maximum number of timesteps have occurred, the simulation terminates. A stable outcome is one in which every agent either (a) has credence > .99 that action B is correct or else (b) has credence low enough that, even upon seeing evidence that action B is better, they will disregard the evidence. The community may end up in a state of correct consensus (all scientists believe action B is better than action A), incorrect consensus (all scientists believe A is better than B), or polarization. 

{% include figure image_path="/assets/images/polarization/example-run.gif" alt="" caption="An example of the simulation." %}

**Code optimization:** Initially, the simulation ran quite slowly. The calculation of how much to update one’s credence based on a given neighbor’s evidence is computationally intensive. However, since the function is only dependent on the scientists’ starting credence, their neighbor’s credence, and their neighbor’s evidence (represented by how many successes they saw in _n_ trials), I was able to save these “update values” in a matrix so they’ll only need to be calculated once at the beginning of the simulation. Credences were discretretized by separating them into 20 bins. These update values are persistent, so even if you run the simulation multiple times, they’ll only need to be recalculated if the parameters are changed. These improvements reduced CPU time by about a factor of 10.

## Results

Let's take a look at how varying the parameters of the model affected outcomes.

| Parameter | Definition | Range |
|-------|--------|---------|
| $$P_B$$ | The probability that action B succeeds. This is compared to that of action A, which has $$P_A$$=.5. | .5001-.8 |
| $$m$$ | The multiplier that determines how quickly scientists begin to mistrust those with different beliefs. | 1-3 |
| $$degree$$ | The size of each scientist's neighborhood. A measure of community connectedness. | 1-All Scientists |
| $$n$$ | The number of tests each scientists runs in every round of their experiments. | 1-100 |
| $$dim$$ | $$dim^2$$ is the number of scientists in the community. | 1-10 |

Increasing the size of the scientists' neighborhoods increases the probability that any given scientist will converge to the correct belief. Increasing the distance between $P_B$, the probability of success of the better action B, and $P_A$, the probability of success of the acceptable action A, also predictably leads to more scientists finding action B is better. Increasing $m$, however, decreases the proportion of correct individuals, as scientists more strongly distrust the evidence of others with dissimilar beliefs.

{% include figure image_path="/assets/images/polarization/CvsDvsPvsM.svg" caption="Simulations had $dim=4$, $n$=20. For each graph, the other parameters were uniformly sampled over their range."%}

The $degree$ and $P_B$ parameters don't strongly interact. However, since $m$ determines how scientists react to the evidence of others, its effect is less pronounced when the degree is smaller and scientists have fewer neighbors.  

{% include figure image_path="/assets/images/polarization/CvsDP.svg" caption="Averaged over 50 trials with $m = 1.5$, $dim=4$, $n$=20."%}
{% include figure image_path="/assets/images/polarization/CvsDM.svg" caption="Averaged over 50 trials with $P_B = .8$, $dim=4$, $n$=20."%}

Adjusting the parameters also affects the probability that a community will reach consensus or polarize. Increasing $degree$ leads to more and community consensus to the right answer as well as slightly more polarization. Notice that consensus on the wrong answer is sometimes more common than consensus on the right answer. Remember that in this simulation, those who believe, incorrectly, that action A outperforms action B will no longer perform their own experiments to collect evidence on action B. Although no longer investing time and energy to investigating a "worse" option is generally a rational thing to do, it still allows for consensus on the incorrect answer to arise more easily. 

{% include figure image_path="/assets/images/polarization/statevsD.svg" alt="" %}

Again, we see that varying $m$ is not as significant when $degree$ is small. $P_B$ is also more significant when $degree$ is small, and communities are more susceptible to consensus to the wrong answer.

{% include figure image_path="/assets/images/polarization/polarvsDP.svg" alt="" %}
{% include figure image_path="/assets/images/polarization/polarvsDM.svg" alt="" %}

Did scientists tend to form in close-knit pockets, or were they well-dispersed over the space? We can measure how blended a group of scientists is by checking the entropy. The entropy of a purely random initial map will be around 3. Here we see that, over time, entropy decreases and the communities become more homogenous. It's only when $degree=1$, when scientists are not considering the evidence of their neighbors at all, that entropy actually increases as the scientists become more firmly set in their beliefs. 
{% include figure image_path="/assets/images/polarization/entropyvsT.svg" alt="" %}

## Conclusions

Discarding evidence of those deemed to be untrustworthy sources can be a mechanism for the emergence of polarization. In these simulations, considering the evidence of others led to greater probabilities of believing in the correct answer, with benefits tapering off significantly after 7 or more neighbors' evidence was considered. 

## References

[^1]: O'Connor, Cailin & Weatherall, James Owen (2017). Scientific polarization. _European Journal for Philosophy of Science_ 8 (3):855-875. https://philpapers.org/rec/OCOSP 
