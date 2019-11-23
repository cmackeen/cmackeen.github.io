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
## Aggregating post-edge absorption in an interactive periodic table
11/22/2019
{: .fw-300 }

I have stepped away from the hEXAFS project for some months, and recently rolled up the sleeves and began digging through the attempts at naive fourier transforms I used to apply to all of the postedge x-ray absorption (XAS) data. At that point I found it was a good idea to rigorously extract, reduce, and aggregate the post-edge data  to present it in the periodic table. My strategies rely on simple linear fits of the background, and shifting the edge to be somewhat normalized around $$\mu=$$. Further, I use a linear interpolating function as I plan on applying an FFT and would appreciate normal sampling in energy. Below is a periodic table with a logarithmic heat map of how many samples were used in the displayed average XAS post-edge spectrum. 

{%include ptable_pics.html %}

When hovering over an element, your are viewing an average of the normalized/reduced post-edge spectrum for all of the data Bud collected at that edge (and reduced). There are errors, some more apparent than others, and their source varies from fluctuation of edge energy to variety in samples studied that contain that element. When I was at the beamline watching a scan *slowwwwly* scroll by, I was always interested if the spectrum was being collected properly. How could I tell?  Experience that I did not yet have. So this is a rough idea of "fingerprints" for the edges studied; some elements should always exhibit a clear white line, and others none at all.

These images and plots will improve as I learn and hone my energy-space in-house data reduction methods, but this is progress worth sharing.

I will err on the side of physics-ignorance. How would I clean and engineer this data if I did not know much about XAS? My overall goal is to use Bud's library of reduced and analyzed EXAFS data to train a model to predict reduction parameters, like the k-window, k-weighting, E-min, and fitting splines. As I learn to aggregate the data and use signal processing techniques to improve FFT analysis, I am left to wonder how much National Labs like Oak Ridge, SLAC, and BNL can benefit from these tools, accelerating user discoveries. Well, on to the next iteration!



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
