---
layout: default
title: Edge Bleed Background
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

# Simulated Pre-edge Subtraction 

{: .no_toc } 

When analyzing the fine structure
in the post edge of a spectrum, it is important to have rigorous background
removal. This is done generally with spline fitting of a slowly oscillating
background, where we can choose the number of knots using the Nyquist-Shannon sampling theorem:   $$ N_{\rm knots}  =  1 + \frac{2R_{\rm bkg} \Delta k}{\pi} $$ .

But there are occasions where another set of EXAFS oscillations of an edge around 300eV below your desired edge contribute to the background. Since this specific background is also EXAFS oscillations, it will not necessarily be filtered out by the generic spline fitting. 


## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---
## Simple yet specialized background subtraction in the pre-edge and beyond
*6/11/2019*
{: .fw-300 }


![](/assets/predge_bleed_91.png)

(source:JB Boyce, FG Bridges, T Claeson, *Local-Structure of BaBi$$_x$$Pb$$_{1-x}$$O$$_3$$ Determined by X-ray Absorption Spectroscopy* (1991) [link](/assets/bridgeboyce91.pdf) )

The procedure followed in 1991 was not designed for batch use, and we have many
temperatures and files to run over, so it is worth setting a framework where
python can optimize a "stencil" fit to the large oscillations in the pre-edge.

As an overview, when looking at the EXAFS part of the spectrum (>100eV above
edge), you may find an second absorption edge at hi $$k$$. Sometimes, it is low
enough in energy that the EXAFS oscillations are not negligible, and maybe you
even want to analyze the second edge as well. 

The tail end of the first absorption edge then needs to be subtracted from the
second edge, as is evident in the remnant oscillations of the higher energy
pre-edge. We approached this problem with experience using FEFF for fitting
real-space EXAFS data, which allows us to simulate the lower energy EXAFS
oscillations out to extremely high $$k$$ ($$\sim 20 \AA$$). Next, we take this
extended simulated standard, and use it functionally as a stencil to try to fit
the second pre-edge. We only optimize parameters to scale the amplitude, and
move the energy. Fitting with this rigid stencil instills faith in our real-space fit procedures and FEFF.

![](/assets/esfit_stenc.jpg)


The general program developed to do this pre-edge bleed data correction is available on [github](https://github.com/cmackeen/exafsbk). It is a bash executed python script and takes different inputs as flags. 
