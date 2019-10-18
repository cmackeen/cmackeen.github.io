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


{% include lib/mathjax.html %}


# Supratmos Systems

Supratmos Systems is the working title for a dirigible project I am pumping
away at. Succinct goal: develop technology for sustainable
autonomous dirigibles. 

## Module Engineering and Geometry of HCP Tetrahedron
What if we did not cover each balloon with a flame-proof system to stymie sympathetic detonation? I first look to hexagonal close-packing and find that packing spherical balloons in a regular tetrahedron may minimize surface area of our flame-proof system. Below I show a model (using babylonjs) of 4 balloons covered by a tetrahedron, with one side removed so that we can peer inside. Go ahead, clock and drag around!

{% include hcp_tetra_3D.html %}


## Payload Analysis round 1
9/25/2019
{: .fw-300 }



The past few months have been touch and go, and I have been engulfed in acquiring materials for the first phase of prototyping. As I rely on simulation and programs to probe what is *worth* pursuing, I am hopeful to pull simulation and reality closer via lightweight flame resistant prototype. 

With regards to simulating I knew my payload estimates must include altitude,
which entails using a rough simulation of absolute pressure and temperature
through the troposphere and stratosphere. With these parameters, I can derive
how the density of air and hydrogen varies at altitude. There are many design
considerations to come, but in this post the model will assume we are starting
with a volume V₀ that will remain fixed via venting hydrogen. Dirigible
technology from 1910-1930s includes innovations for rigid and semi-rigid
airships that include a pressurized envelope around the smaller gas bags.
But we will start with a simple model, because these early attempts focus on the cross-over size when a balloon covered
in a flame proof material/shell can be safe and float. 
2

First, a surface representing payload as a function volume and altitude:



<div class="video-container">
    <iframe src="https://cmackeen.github.io/assets/pload_surface.html" height="700px" width="700px" allowfullscreen="" frameborder="10px">
    </iframe>
</div>


{% include pload_surface.html %}

The above interactive plot is an estimation based on a flameproof shell of 6cm of foam with density = 25kg/m³ . It is clear how dramatically the payload decreases with altitude. This payload, like in the following plots, is simply defined by the net mass of aerostatic unit. More explicitly:

$$payload = V_{bal} ( \rho_{air} - \rho_{H_2} ) - (M_{mylar}+M_{foam}) $$
 

Now, a more concise plot. This plot shows "critical lines" of equivalent payload for different foam thicknesses (red/blue) and payloads (solid/dashed), with the balloon held at constant volume via venting (like before). Bear in mind the y-axis is on a log scale.

{% include critvol_alt_fullmsl.html max-width="80%" %}



With the aformentioned density and thickness, it seems a reasonable estimate to balloon volume for this simplified unit is ~7000 m³ (*r ~ 12m*). This takes into consideration we keep the balloon at constant volume and vent hydrogen to equilibrate pressure. The next post will expound upon tradeoffs of partial filling at sea-level. 


## So, How Much can Hydrogen lift?
6/27/2019
{: .fw-300 }

The following is an interactive plot calculating the payload of a single hydrogen filled balloon, if it were covered in a low density foam shell. The crossing of the x-axis represents the critical radius a balloon needs to generate lift.


{% include payload_slider.html %}

It turns out that a foam coated balloon with a 2 meter radius can lift 10kg, so it only takes maybe 100 of these balloons to lift a metric ton. In considering a craft filled with these balloons in a cylindrical configuration does not necessarily exceed 100m in length, our toy-model hydrogen-ball filled dirigible is not a behemoth. 



## Beginnings
5/20/2019
{: .fw-300}

While I have gotten caught up in speculating how the world will use airships, it started with just one balloon.
I was crew on a sailling passage down the central California coast, and I saw a balloon floating on
top of the water. It seemed like evidence to answer an old question: where do balloons go when they fly away? The ocean! I imagined it flew 100 statute miles, or drifted even further. These natural forces can move a balloon a long ways incidentally, what if you could control it? Like sailing the skies, in some modern dirigible you can harness the power of the wind. 

From that point, I began researching the history and physics of aerostats. The problem with modern aerostat applications is the scarcity of helium, the lifting gas.



Now I *especially* know the cost of helium. I have seen days at the lab where hellium just
isn't delivered to the synchrotron, where it is used for
cryogenic experiments below 77K. Why, you may ask? There just isn't enough, and it isn't
coming back. 


We can look to the vacuum airship, as first described by [Francesco Lana de Terzi](https://en.wikipedia.org/wiki/Airship#/media/File:Flying_boat.png). There is also hydrogen. 

If we look at a less economic but
renewable extraction of hydrogen gas, and assuming electric costs of 12¢/ kWh
(which is high), then commercial electrically synthesized hydrogen costs $$\sim$$ 0.054 ¢/L . The
current commercial rate for grade-A helium is .76 ¢/L , and that is not taking into account it is twice as dense.
[source](https://prd-wret.s3-us-west-2.amazonaws.com/assets/palladium/production/s3fs-public/atoms/files/mcs-2019-heliu.pdf) 

So I think the world needs to turn to non-helium aerostats.
