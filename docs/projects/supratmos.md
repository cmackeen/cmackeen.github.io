---
layout: default
title: Supratmos Systems
nav_order: 3
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

# Supratmos Systems

Supratmos Systems is the working title for a dirigible project I am pumping
away at. General theme of our goal: develop technology for sustainable
autonomous dirigibles. While I have gotten caught up in imagining and
speculating how the world will use airships, it started with just one balloon.
I was sailing and 10 NM off the coast of Big Sur I saw a balloon floating on
top of the water...

Now I *especially* know the cost of helium. I have seen days where it just
isn't delivered to the synchrotron at a national lab, where it is used for
cryogenics below 77K. Why, you may ask? There just isn't enough, and it isn't
coming back. It's expensive as is. From looking at the less economic but
renewable extraction of hydrogen gas, and assuming electric costs of 12¢/ kWh
(which is high), then it commercial electric costs $$\sim$$ .054 ¢/L . The
current commercial rate for grade-A helium is .76 ¢/L , and that is not taking into account it is twice as dense.
[source](https://prd-wret.s3-us-west-2.amazonaws.com/assets/palladium/production/s3fs-public/atoms/files/mcs-2019-heliu.pdf) 

So I think the world needs to turn to non-helium aerostats.



## So, How Much can Hydrogen lift?
6/27/2019
{: .fw-300 }

The following is an interactive plot calculating the payload of a single hydrogen filled balloon, if it were covered in a low density foam shell. The crossing of the x-axis represents the critical radius a balloon needs to generate lift.


{% include payload_slider.html %}

It turns out that a foam coated balloon with a 2 meter radius can lift 10kg, so it only takes maybe 100 of these balloons to lift a metric ton. In considering a craft filled with these balloons in a cylindrical configuration does not necessarily exceed 100m in length, our toy-model hydrogen-ball filled dirigible is not a behemoth. 
