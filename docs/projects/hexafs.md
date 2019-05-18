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
## A first look at historical EXAFS
*5/12/2019*
{: .fw-300 }

The X-ray absorption spectra Bud Bridges has collected is atom specific, so we can get an overview of what elements were looked at over 30 years. Later on we can deduce specific compounds using both metadata and statistical modeling. But for now, we get a quick look at what energies Bridges lab collected data at.

To do this, I used a couple of filters and so currently this may be pulling
from roughly a quarter of all the normalized e-space EXAFS data. A chunk of
data are also lost to the low sensitivity of my absorption edge identitifier. 
 With a 7eV sensitivity for element matching with energy of maximal derivative of the spectra, we leave half of the 36,000 normalized spectra unidentified.

Either way, below you can find a colored periodic table. When you hover on an
element, you can see how may data scans have been collected. 

{%include ptable_trend2.html %}


Evidently, Bud has a lot of history with manganates. The next post will feature the effectiveness of my edge detection, and how to improve it without running through all ~50,000 absorption spectra.