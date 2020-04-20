---
layout: default
title: Bitcoinj
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

# Pilemma 

{: .no_toc } 

Pilemma is a deep reinforcement learning project with the goal of AI powered penetration testing. Specifically, Pilemma will strive to audit incentive structures for their potential weaknesses, and certify robust protocols for user safety and in turn adoption.  

I have been generally interested in blockchain technology like bitcoin for five years now. Although I have done my share of armchair research into bitcoin and decentralized systems, I still lack technical understanding. With free time, a mentor, and a desire to contribute to open source code, I have started to work on bitcoinj. I will let the [BitcoinJ website](https://bitcoinj.github.io/) explain the details; from what I understand BitcoinJ is a java library that underly many applications that interact with the bitcoin blockchain. Continued development on BitcoinJ will generate more possibilities for what a blockchain app can do.

For now, I am getting re-familiarized with compiled languages and testing Java applications for the first time. My current task is to clean up a bunch of javadoc exception errors. The joy of learning!


---



