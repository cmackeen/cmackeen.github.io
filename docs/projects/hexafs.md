---
layout: default
title: hEXAFS
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


# Historical EXAFS
{: .no_toc }

The Bridges lab in the Physics department at University California, Santa Cruz specializes in local structure characterization using X-ray absorption techniques. Bud Bridges has collected and analyzed high quality EXAFS data for over 30 years; now it is time to learn from it all! This is a step in an effort to automate certain parts of this specialized analysis and uncover correlations between features of the materials studied and their absorption spectra.

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---
## General characteristics of spectra
6/27/2019
{: .fw-300 }

As the data is supposed to be normalized, I thought comparing the post-edge mean and standard deviation would be valuable. 
First plot here is the distribution of extracted means of the post-edge region. Interestingly the main distribution centered near 1 seems to have a bimodal feature.

{% include post_edge_mean_mu.html %}

And here is the distribution of standard deviation in the post-edge area


{% include post_edge_std_mu.html %}

I can see several modes in the distribution of mean aborption, and I think it would be helpful to render a new interactive periodic table plot where the hover includes information of each element's mean aborption in the EXAFS region. Along with that, I will include mena standard deviation.

## A first look at historical EXAFS
5/12/2019
{: .fw-300 }

The X-ray absorption spectra Bud Bridges has collected is atom specific, so we can get an overview of what elements were looked at over 30 years. Later on we can deduce specific compounds using both metadata and statistical modeling. But for now, we get a quick look at what energies Bridges lab collected data at.

To do this, I used a couple of filters and so currently this may be pulling from roughly a quarter of all the normalized e-space EXAFS data. A chunk of data are also lost to the low sensitivity of my absorption edge identitifier. 

Either way, below you can find a colored periodic table. When you hover on an
element, you can see how may data scans have been collected. It's a start !!

{%include ptable_trends.html %}


Evidently, Bud has a lot of history with manganates. The next post will feature the effectiveness of my edge detection, and how to improve it without running through all ~50,000 absorption spectra.
