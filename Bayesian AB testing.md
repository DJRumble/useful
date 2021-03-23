# Bayesian AB testing

 A/B testing is a statistical design pattern for determining the difference of effectiveness between two different treatments.

EXAMPLE:  front-end web developers are interested in which design of their website yields more sales...

Assume that there is some true 0< _pA_ <1 probability that users who, upon shown site A, eventually purchase from the site. Currently, this quantity is unknown to us.

We can know the...
- Observed frequency of sales (empirically)
- True frequency of a sale (often hidden)

In this test we want to infer the true frequncy,  _pA_, from the observed frequency by looking at _S_ number of  _coversions_  over _N_ number of  _trials_.

For example,
- NA = 1300
- NB = 1275
- SA = 120
- SB = 125

Since we are modeling a probability, a good choice for a prior distribution is the Beta distribution. (Why? It is restricted to values between 0 and 1, identical to the range that probabilities can take on.)

### Prior - Beta(α0, β0)

where..
- α0 = prior estimte of conversions successes
- β0 = prior estimate of conversion failures

### Posterior - Beta(α0 + S, β0 + N − S)

In code this looks like...
```
from scipy.stats import beta
conversions_from_A = 120
conversions_from_B = 125
visitors_to_A = 1300
visitors_to_B = 1275

alpha_prior = 1
beta_prior = 1

posterior_A = beta(alpha_prior + conversions_from_A,
                    beta_prior + visitors_to_A - conversions_from_A)
posterior_B = beta(alpha_prior + conversions_from_B,
                    beta_prior + visitors_to_B - conversions_from_B)
```

Which is more succesful? Use samples from the posterior and compare the probability that samples from the posterior of A are larger than samples from the posterior of B.
 

```
samples = 20000 # We want this to be large to get a better approximation.
samples_posterior_A = posterior_A.rvs(samples)
samples_posterior_B = posterior_B.rvs(samples)
items = (samples_posterior_A > samples_posterior_B)
print(np.mean(list(map(lambda x: float(x), items))))

[Output]:
0.31355
```
They way to interpret this result is to say this result demonstrates a 31% chance that site A converts better than site B. Therefore less than one in three samples A will be better (this is not very good).

### Vizualise results

```
%matplotlib inline
from IPython.core.pylabtools import figsize
from matplotlib import pyplot as plt
figsize(12.5, 4)
plt.rcParams['savefig.dpi'] = 300
plt.rcParams['figure.dpi'] = 300

x = np.linspace(0,1, 500)

plt.plot(x, posterior_A.pdf(x), label='posterior of A')
plt.plot(x, posterior_B.pdf(x), label='posterior of B')
plt.xlabel('Value')
plt.ylabel('Density')
plt.title("Posterior distributions of the conversion
           rates of Web pages $A$ and $B$")
plt.legend();

```
 
# Bayesian methods

For (almost) every inference problem there is a  **Bayesian method** and a  **frequentist method** . The significant difference between the two is that Bayesian methods have a prior distribution, with hyperparameters α, while empirical methods do not have any notion of a prior.

## The basics

To align ourselves with traditional probability notation, we denote our belief about event A as P(A). We call this quantity the  _prior probability_ .A Bayesian updates his or her beliefs after seeing evidence. We denote our updated belief, or  _posterior probability_ , as P(A|X), interpreted as the probability of A given the evidence X.By introducing prior uncertainty about events, we are already admitting that any guess we make is potentially very wrong. After observing data, evidence, or other information, we update our beliefs, and our guess becomes less wrong.

Traditional Bayesian inference - prior ⇒ observed data ⇒ posterior
Empirical Bayes - observed data ⇒ prior ⇒ observed data ⇒ posterior

As the amount of our observations or data increases, the influence of the prior decreases.

### Subjective vs Objective priors

Bayesian priors can be classified into two classes:
- objective priors, which aim to allow the data to influence the posterior the most. For example, uniform distribution with large bounds.
- subjective priors, which allow the practitioner to express his or her views into the prior. For example, Normal random variable with large variance (small precision) might be a better choice, or an Exponential with a fat tail in the strictly positive (or negative) case.
 
Specifying a subjective prior is how practitioners incorporate domain knowledge about the problem into our mathematical framework. Allowing domain knowledge is useful for many reasons, for example:

- Aids the speed of MCMC convergence
- More accurate inference.
- Express our uncertainty better.

 
#### Useful Priors

Here are some common PDFs fpr use as a prior.

- [Gamma distribution ](https://en.wikipedia.org/wiki/Gamma_function) - ```y =stats.gamma.pdf(x, a, scale=1. / b)```
- [Wishart distribution](https://en.wikipedia.org/wiki/Wishart_distribution) - ```pymc.rwishart(n + 1, np.eye(n))```
- [Beta distribution](https://en.wikipedia.org/wiki/Beta_function) - ```stats.beta.pdf(x, a, b)```

Beta distribution is a generalization of the Uniform distribution, something we will revisit many times.
The module pyMC3 contains many more examples of common distributions, see for [documentation](https://docs.pymc.io/api/distributions/continuous.html).

#### Regret


Total regret is a  a metric to calculate how well a strategy is doing.

Some example startegies that are available in ```other_strats.py``` are...
- random
- largest bayesian credible bound
- Bayes-UCB algorithm
- Mean of Posterior
- Largest proportion
 