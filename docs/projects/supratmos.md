---
layout: default
title: Supratmos Systems
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
away at. Succinct goal: develop technology for sustainable autonomous
dirigibles. While I have gotten caught up in speculating how the world will use
airships, it started with just one balloon.  I was crew on a sailling passage
down the central California coast, and I saw a balloon floating on top of the
water. It seemed like evidence to answer an old question: where do balloons go
when they fly away? The ocean! I imagined it flew 100 statute miles, or drifted
even further. These natural forces can move a balloon a long ways incidentally,
what if you could control it? Like sailing the skies, in some modern dirigible
you can harness the power of the wind. 

From that point, I began researching the history and physics of aerostats. The
problem with modern aerostat applications is the scarcity of helium, the
lifting gas.



Now I *especially* know the cost of helium. I have seen days where
helium isn't delivered to the synchrotron, and cryogenic experiments must be postponed. There simply isn't enough helium, and it isn't coming back. 


We can look to the vacuum airship, as first described by [Francesco Lana de
Terzi](https://en.wikipedia.org/wiki/Airship#/media/File:Flying_boat.png).
There is also hydrogen. 

If we look at a less economic but renewable extraction of hydrogen gas, and
assuming electric costs of 12¢/ kWh (which is high), then commercial
electrically synthesized hydrogen costs $$\sim$$ 0.054 ¢/L . The current
commercial rate for grade-A helium is .76 ¢/L , and that is not taking into
account it is twice as dense.
[source](https://prd-wret.s3-us-west-2.amazonaws.com/assets/palladium/production/s3fs-public/atoms/files/mcs-2019-heliu.pdf) 

So I think the world needs to turn to non-helium aerostats.



## So, How Much can Hydrogen lift?
6/27/2019
{: .fw-300 }

The following is an interactive plot calculating the payload of a single
hydrogen filled balloon, if it were covered in a low density foam shell. The
crossing of the x-axis represents the critical radius a balloon needs to
generate lift.


{% include payload_slider.html %}

It turns out that a foam coated balloon with a 2 meter radius can lift 10kg, so
it only takes maybe 100 of these balloons to lift a metric ton. In considering
a craft filled with these balloons in a cylindrical configuration does not
necessarily exceed 100m in length, our toy-model hydrogen-ball filled dirigible
is not a behemoth.

How does this compare to a cargo ship (you may be asking)? A 20 ft cargo
container can have a full mass of 25 metric tons; one ship may carry a thousand
of these containers (tought to match). But back to the single container --it
costs roughly $2500 to ship it, so $500/1000kg, or 50 cents per kilo. The
lifting gas costs us roughly .167 ¢/kg in our model with 2 meter radius
balloons bunched together. 

Of course, the next step is factoring the cost of
the vessel including on board AI, comms, mylar and aluminum. But that is too
far ahead, simpler yet crucial decisions are coming fast. 
