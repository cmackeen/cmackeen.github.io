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
protocols for user safety and in turn adoption.  


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

## MakerDAO Dai Collateral Debt Positions -> simplified.

While the system behavior is quite complex (and possibly unstable), my
job is to create a reward structure and game environment where an agent can
optimize a policy.

To open a Collateral Debt Position (CDP) we lock ethereum in a contract (ETH)
and receive Dai, MakerDAO`s stablecoin. This is analogous to "minting" Dai, and
now to get the collateral ETH back (from the "urn") you must return the Dai you
minted. While your CDP is open, interest accruing on the debt (the "stability
fee") will be deducted from your collateral ETH.  If at anytime your CDP "urn"
of ETH falls below 150% in value (Dai conversion based on some oracle) compared
to the Dai minted, you hit the "liquidation ratio". This triggers events which
in turn lead to the liquidation of some or *all* of your "urn".  

![Dai CDP minting and auction cycle](/assets/DaiCDP.svg)

The way this auction (we will talk about the "dent" portion in particular)
means that "auction keepers" will bid on how *little* ETH they are willing to
receive for returning the outstanding Dai debt on that "vault". Each bid goes
down by increments of 3%, and all are welcome to participate. 

Now if you're sharp , then you may have reached the idea that you could open
barely liquid CDP's and bid on your own "urn" once you fall below the
liquidation ratio. Well this would work if there was not a liquidation fee of
13%.  Why is a 13% enough to stop you from this auction-grind exploit? Well,
that is why we are here! 

 In the future, the entire decentral protocol can be run with multi-agent
environments in search of exploits.
## Past work in blockchain tech

### BitcoinJ

I have been generally interested in blockchain technology like bitcoin for five
years now. Although I have done my share of armchair research into bitcoin and
decentralized systems, I still lack technical understanding. With free time, a
mentor, and a desire to contribute to open source code, I have started to work
on bitcoinj. I will let the [BitcoinJ website](https://bitcoinj.github.io/)
explain the details; from what I understand BitcoinJ is a java library that
underly many applications that interact with the bitcoin blockchain. Continued
development on BitcoinJ will generate more possibilities for what a blockchain
app can do.

For now, I am getting re-familiarized with compiled languages and testing Java
applications for the first time. My current task is to clean up a bunch of
javadoc exception errors. The joy of learning!



---



