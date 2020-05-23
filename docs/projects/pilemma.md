---
layout: default
title: Pilemma
nav_order: 5
parent: Projects
---
<button class="btn js-toggle-dark-mode">~Nightmode~</button>

<script>
const toggleDarkMode = document.querySelector('.js-toggle-dark-mode')
const cssFile = document.querySelector('[rel="stylesheet"]')
const originalCssRef = cssFile.getAttribute('href')
const darkModeCssRef = originalCssRef.replace('just-the-docs.css', 'dark-mode-preview.css')

addEvent(toggleDarkMode, 'click', function(){
  if (cssFile.getAttribute('href') === originalCssRef) {
    cssFile.setAttribute('href', darkModeCssRef)
  } else {
    cssFile.setAttribute('href', originalCssRef)
  }
})
</script>


{% include lib/mathjax.html %}

![](/assets/pilemma_logo_small.jpg)
{: .no_toc } 

Pilemma is a deep reinforcement learning project designed to exploit
decentralized incentive systems. Specifically, Pilemma's objective is to audit
incentive structures for their potential weaknesses, and certify robust
protocols for user safety and in turn, safe adoption.  

# Observation Space in Decentralized Finance

Enriching a learning agent's observations is the feature engineering task at hand: what can an agent learn from efficiently? The agent needs to learn novel behavior *quickly*, so we must feed it the most pertinent information.

Since I am focusing on augmenting the observation (state) space for robot, I am implementing a streaming anomaly detection on transaction data, [see viz here!](http://ec2-54-89-50-233.compute-1.amazonaws.com:8080/) This graph employs a [MIDAS](https://towardsdatascience.com/anomaly-detection-in-dynamic-graphs-using-midas-e4f8d0b1db45) anomaly score to filter out negligible background acitivity, saving CPU time and enabling scalability. Aside from MIDAS, the data is brought to you by web3 endpoint [Quicknode](https://www.quiknode.io/), Web3py, Dash Cytoscape, and a streamlined deque-write:gunicorn-reload process. That means the graph is current (last 1 million tx's filtered) and reloaded every 10 minutes, but when you refresh the browser it should not take long to reload unless the data is new. 

The visualisation will help me (and others) understand what's worth the time when it comes to processing transactional graph features. I do not want to inundate the agent with transaction data, but it should be informed when activity between two DeFi exchanges is spiking. 

# First Application

The current (and first) incentive structure being tested is the MakerDAO
multi-collateral debt protocol, Dai. During a market drop of ethereum`s price
in mid march 2020, many debt positions were liquidated and collateral ethereum
sold to "auction keepers". Due to network congestion and auction conditions, some
ETH collateral was sold (AKA "flipped") at auction with nearly zero bids. 

This event is another example of exploits in decentral protocols. The future of
decentralized finance (DeFi) depends on more rigorous and quantitative testing.
This is where I will apply AI and Reinforcement Learning to answer tough
questions: how robust is your system to bad actors? 

![Dai CDP minting and auction cycle](/assets/makerdao.svg)

## MakerDAO Dai Collateral Debt Positions -> simplified.

While the system behavior is quite complex (and possibly unstable), my
job is to create a reward structure and game environment where an agent can
optimize a policy.

To open a Collateral Debt Position (CDP) we lock ethereum in a contract (ETH)
and receive Dai, MakerDAO`s stablecoin. This is analogous to "minting" Dai, and
now to get the collateral ETH back (from the "urn") you must return the Dai you
minted. While your CDP is open, interest accruing on the debt (the "stability
fee") will added to the amount of Dai it takes to retrieve your collateral.  If at anytime your CDP "urn"
of ETH falls below 150% in value (Dai conversion based on an oracle) compared
to the Dai minted, you hit the "liquidation ratio". This triggers events which
in turn lead to the liquidation of some or *all* of your "urn".  

<p align="center">
  <img  src="/assets/DaiCDP.svg">
</p>


The way this auction (we will talk about the "dent" portion in particular)
means that "auction keepers" will bid on how *little* ETH they are willing to
receive for returning the a portion of the outstanding Dai debt on that urn. Each bid goes
down by increments of 3%, and all are welcome to participate. 

Now if you're sharp , then you may have reached the idea that you could open
barely liquid CDP's and bid on your own "urn" once you fall below the
liquidation ratio with an amount you consider fair value (and no more). This auction grinding strategy is outlined [in this document](https://github.com/livnev/auction-grinding/blob/master/grinding.pdf). Well this would work if there was not a liquidation fee of
13%.  Why is a 13% enough to stop you from this auction-grind exploit? Well,
that is why we are here! 

 In the future, an entire decentral protocol or smart contract can be run with multi-agent
environments in search of exploits.
## Reinforcement learning in context

There are many intriguing and complex algorithms rapidly evolving in reinforcement learning, but I choose to focus on policy based learning. It is also important to note that the problem formulation determines both the applicability of RL and the success. In general, delayed gratification markov decision processes are opportune systems to employ RL.


The initial attack plan consists of a single agent environment with 5 action choices: buy ETH (with Dai), sell ETH, mint Dai, close CDP, and skip. The most simplified environmental observations include only the open, high, low, close of 1 minute ETHUSDT data. My protoype learning environment is an OpenAI gym format and works with PPO2 stable baselines policy learning. I am currently re-factoring into an rllib MultiAgentEnv to include adversarial probabilistic bidding to more closely simulate auction processes. The project's code can be found in my [github here.](https://github.com/cmackeen/pilemma)

## What an Agent does
In this simple example I trained an agent on a 1000 minute time series of raw financial data. If the agent sees the same observations over and over again, it can learn to maximize reward (delta portfolio) while using minimum actions. The plot below is an example episode fromt trained agent.

{% include candlesplin.html %}

But if we want it to learn well on the fly, we must augment the agent observation data. The following is an overview of the RL architecture for a single agent learning to exploit a smart contract environment. As subgraphs ([GraphQL](https://thegraph.com/)) and  analytics tools ([Dune](https://www.duneanalytics.com/)) gain popularity, a standardized (and simplified) set of contract observations and actions will emerge, all attached to simple human readable queries.

![Single Agent RL Workflow](/assets/pilemma_over.png)


## Past work in blockchain tech

### BitcoinJ

I have been generally interested in blockchain technology like bitcoin for five
years now. Although I have done my share of armchair research into bitcoin and
decentralized systems, I still lack technical understanding. With free time, a
mentor, and a desire to contribute to open source code, I have started to work
on bitcoinj. I will let the [BitcoinJ website](https://bitcoinj.github.io/)
explain the details; BitcoinJ is a java library that
underly many applications that interact with the bitcoin blockchain. Continued
development on BitcoinJ will generate more possibilities for what a blockchain
app can do.

For now, I am getting re-familiarized with compiled languages and testing Java
applications for the first time. My current task is to clean up a bunch of
javadoc exception errors. The joy of learning!



---



