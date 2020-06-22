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

<picture>
    <source srcset="/assets/pilemma_logo_small_inv.jpg" media="(prefers-color-scheme: dark)">
    <img src="/assets/pilemma_logo_small.jpg">
</picture>


![](title_pic)
{: .no_toc } 

Pilemma is a deep reinforcement learning project designed to exploit
decentralized incentive systems. Specifically, Pilemma's objective is to audit
incentive structures for their potential weaknesses, and certify robust
protocols for user safety and in turn, safe adoption. 

# Graph Convolutions: Discovery of Unlisted yet Active Ethereum Tokens

On route to augmenting an RL agent environment, I have found myself rigorously modeling transaction graph data on the ethereum network. Since transaction data is represented well in graph form, I looked to employ a machine learning method that exploits the connected structure without manually building large input tensors and eating memory. My goal was simple: by only looking at a static snapshot of transaction activity (edges), I wanted to identify addresses (nodes) that were home to token contracts. I could label nodes by using the [Coingecko API](https://www.coingecko.com/en/api), and train a graph convolutional neural network for binary classification.

Below we can see an example how graph structure provides rich information. These two plots represent transaction graphs of two different listed tokens, including all 2nd neigbor interactions. The node color corresponds to activity as calculated using pyMIDAS. 

{% include token_graph_x2.html %}

## Aggregation Methods

My [website](http://pilemma.com) builds from a constantly updating python dequeu of the previous 300k transactions, and each edge has an anomalous activity index via [pyMIDAS](https://github.com/Stream-AD/MIDAS), so this our starting point for node features. Since I aim to have weightless edges and featured nodes, I average each node's edges to give a mean node acitivity index. This will be the node's first feature, and the second will be the standard deviation of edge anomalous activity indices. 

![](/assets/edge_avg_node.svg)

The last set of features will be a vector representing function calls to the contract. To extract the JSON dictionary of function calls a contract receives, I pull the contract ABI to decode the transaction input bytecode. The ABI simply returns a JSON format dictionary of expected function calls that the bytecode would reference when executing. Through ~300k transactions, I saw 37 unique Solidity methods (`transfer` was popular). I manually rigged up a custom word2vec implementation that leaves each node with an additional sparse 37 features. If the transaction called no method (simple ETH tx), the entire vector is an array of 0.0 . 

The targets are `1` if an address was a token contract listed on coingecko.com, and `0` if not. Out of ~70k nodes in this exercise, ~500 are listed on coingecko, which is a great transition into our next topic on class imbalance.

## Cluster GCN and Classification

This project had me concerned of two things: our targets are greatly imbalanced and our technique is a bit new/foreign. With this in mind, I intended to favor recall over precision. The tools in this rapidly growing space are accesible and powerful. I am fortunate for the development of the [StellarGraph](https://stellargraph.readthedocs.io/en/stable/) package, as it integrates cutting edge graph neural network algorithms seemlessly with Keras and Tensorflow. To start I compiled my graph with feature nodes and saved it in graphml format --loading this into a stellargraph graph is trivial.

A Grapch CNN can simply be understood from [this overview](https://tkipf.github.io/graph-convolutional-networks/) of a 2016 paper. The recursive rule defining the layer sin a GCN is the following:

$$ H^{(l+1)}=f(H^{(l)}, A) = \sigma\left( \hat{D}^{-\frac{1}{2}}\hat{A}\hat{D}^{-\frac{1}{2}}H^{(l)}W^{(l)}\right) \, , $$ 

where $$H^l$$ represents the $$l^{th}$$ hidden layer , $$\hat{A} = A + I
$$ with $$A$$ as the adjacency matrix, and $$\hat{D} = D + I
$$ with $$D$$ as the diagonal node degree matrix. The identity matrices are added to include the origin node's features, 
acting as an effective self-loop. At each layer of depth, the feature dimensions are reduced by the weights $$\gamma \times \gamma^{'}$$ dimensions, and normalzied contributions from neighboring nodes are included.

The ClusterGCN simplifies the GCN in that it limits the neighborhood expansion which can save a great deal of memory, see [the paper](https://arxiv.org/abs/1905.07953) and below image for clarification. The left is an example of normal GCN neighborhood expansion, while the right limits the contributions to a local cluster.


![](/assets/clustergcn.jpg)

Finally, the class imbalance can be adressed most simply by tinkering with weighted binary cross-entropy. I did not go the route of over/undersampling because it is slightly more complicated when it comes to graph data. Plus, it is easier to iterate with the cross entropy weight than resampling subgraph clusters. For good measure I also intialized the ouput layer bias to $$log(pos/neg)$$ to reflect the scarcity of `1` classes. 


## Results

I trained the Cluster GCN over 400 epochs with a schedule to decay the learning rate. The testing metrics for the model are as follows:

| Metric   |      Value    |
|----------|:-------------:|
| Acc      |    0.9593     | 
| Recall   |    0.9178     |  
| Precision|    0.1126     | 


I did not want the model *too* sensitive to missing the `1` classes (listed tokens). I could tell this was the case when recall was maxed to 0.99. The low precision is not as bad as it seems at first glance. To reconnect with my initial goals, I evaluated the model and explored the predictions that were false positives. Indeed, in the FP's I find examples of tokens listed on etherscan but not yet on coingecko. Nearly 10% of the false positives were actually classified tokens but *not* on coingecko, and you can see the table of them [right here](/assets/sub_mf.html) with their associated total supplies in descending scarcity. For the other 90% of false positives, I have empirically found that some are addresses used heavily in concert with a token address. 

### So what...

Well, this feature-lean and computationally economic procedure has identified 271 unlisted (on coingecko) yet currently active tokens. I strongly believe it can be tuned to predict the tokens that get [added to coingecko](https://www.coingecko.com/en/coins/recently_added) from just a time-independant snapshot of the last 300k ETH transactions. While there exist ways to abuse the etherscan and coingecko api limits to find nascent token contracts *without* my ML overkill, let's talk about next steps for applying GCN to ethereum transaction data.

## Conclusion

This exercise lays solid groundwork on what we can learn from algorithmically massaging the ethereum transaction data. If a node acts, talks, and walks like a coingecko listed token, it may be a coingecko listed token. If not now, then sometime *soon*. 

Next comes the integration of coarse grained time dependance. By this I mean using multiple graph snapshots (over time) to predict future token characteristics. Instead of simple classification of a token being listed or not, we can predict a token's ROI for the upcoming month or week. On this idea I am excited, as I think it will also begin to quantify properties of **ponzi tokens** that are different from organic token growth. 

In the open source and decnetralized community, we all get the privilege to access code and data. Blockchain data can be incredibly rich and powerful but it's what you do with it that sets you apart.

![](/assets/geometricdevil.svg)

# Observation Space in Decentralized Finance

Enriching a learning agent's observations is the feature engineering task at hand: what can an agent learn from efficiently? The agent needs to learn novel behavior *quickly*, so we must feed it the most pertinent information.

![Ethereum Transaction Graph](/assets/graph.jpg)

Since I am focusing on augmenting the observation (state) space for the robot, I am implementing a streaming anomaly detection on transaction data, [see graph here!](http://www.pilemma.com) This graph employs a [MIDAS](https://towardsdatascience.com/anomaly-detection-in-dynamic-graphs-using-midas-e4f8d0b1db45) anomaly score to filter out negligible background acitivity, saving CPU time and enabling scalability. Aside from MIDAS, the data is brought to you by web3 endpoint [Quiknode](https://www.quiknode.io/), Web3py, Dash Cytoscape, and a streamlined deque-write:gunicorn-reload process. That means the graph is current (last 200k tx's filtered) and reloaded every 10 minutes, but when you refresh the browser it should not take long to reload unless the data is new. 

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



