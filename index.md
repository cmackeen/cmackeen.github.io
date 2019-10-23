---
layout: default
title: Home
nav_order: 1
description: "Cameron Mackeen -- online "
permalink: /
---


<button class="btn js-toggle-dark-mode">~toggle Night/Day~</button>

<script>
const toggleDarkMode = document.querySelector('.js-toggle-dark-mode')
const cssFile = document.querySelector('[rel="stylesheet"]')
const originalCssRef = cssFile.getAttribute('href')
const darkModeCssRef = originalCssRef.replace('just-the-docs.css', 'dark-mode-preview.css')

cssFile.setAttribute('href',  darkModeCssRef)

addEvent(toggleDarkMode, 'click', function(){
  if (cssFile.getAttribute('href') === originalCssRef) {
    cssFile.setAttribute('href',  darkModeCssRef)
  } else {
    cssFile.setAttribute('href', originalCssRef)
  }
})
</script>
# Welcome


![](/assets/cam_precipice.jpg)

*quick links*: &nbsp; &nbsp;
[Supratmos](/docs/projects/supratmos.md){: .btn .btn-purple .mr-2 }
[Résumé](/assets/cam_mackeen_v3_resume.pdf){: .btn .btn-blue .mr-2}
[Papers](/docs/academic/academic.md){: .btn .btn-blue .mr-2}




 I make the best use of my time by following curiousity and focusing on
projects of passion. This website serves as my interface with the world to
share content related to my growth and experience, and if *anything* here
sparks thoughts worth sharing, please email me at cammackeen@gmail.com. 



# Thanks for visiting 

{: .fs-9 }

{: .fs-6 .fw-300 }





### Active interests:

Artificial Intelligence, Autonomy (human and otherwise), Gardening, Machine Learning, Treking, Physics,  Heliostats, Dirigibles and Aerostatics,
Programming, Music Production and Recording,  Bitcoin, Decentralization, Sailing, Wireless Communication, Shortwave Radio, the Internet 

