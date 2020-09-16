---
layout: post
title: "The Gaussian Distribution"
---
The standard Gaussian distribution is plotted below, where "standard" indicates a mean of 0 and a standard deviation of 1 $$\rightarrow \mu = 0, \sigma = 1$$. Note that the Gaussian distribution doesn't directly give probabilities of outcomes, but rather *relative likelihoods*, meaning you can see which outcomes are more likely than other.

When throwing darts, the dart board is a continuous sample space in which there are an infinite and uncountable number of possible outcomes. This means there is ZERO probability of landing your dart on any particular point. For this reason, for continuous sample spaces we must consider the probability of a *range* of outcomes.

In terms of the (normalized) Gaussian distribution, the probability an outcome is between $$a$$ and $$b$$ is given by the integral of the curve evaluated from $$x = a$$ to $$x = b$$, which gives an area. As such, the total area under the curve is always 1 (i.e. 100% probable). Change the parameter $$n$$ below to find the probability of getting an outcome that is $$n$$ standard deviations from the mean. What happens when the standard deviation of a Gaussian distribution goes to 0? Here's the [answer](https://en.wikipedia.org/wiki/Dirac_delta_function).
<iframe src="https://www.desmos.com/calculator/hizefimwtr" height="700em" width="1000em" style="border: 1px solid #ccc" frameborder=0 class="embed">
</iframe><br>
