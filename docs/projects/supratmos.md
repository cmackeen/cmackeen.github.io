---
layout: default
title: Supratmos Systems
nav_exclude: false
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

<div style='position:relative; padding-bottom:calc(56.25% + 44px)'><iframe src='https://gfycat.com/ifr/WickedDifferentDeer' frameborder='0' scrolling='no' width='100%' height='100%' style='position:absolute;top:0;left:0;' allowfullscreen></iframe></div>

## Module Engineering and Geometry of HCP Tetrahedron
10/18/2019
{: .fw-300 }
As a preamble: in the yet-to-be published Payload Analysis round 2, I explore partial filling of balloons at sea level. In the 1st round of analysis my model was based on slight pressurization (*very* slight) of a balloon at mean sea-level (MSL), with constant one-way venting of hydrogen to maintain near-equilibrium pressure; this leads to a constant volume. I explored (but have not shared) what payload vs altitude looks like when you partially fill the baloon, so it expands in volume upon rising. I aim to have expansion until around 9km altitude, and I found this can be done with ~40% initial fill. Moving forward....

What if we did not cover each balloon with a flame-proof system to stymie sympathetic detonation? I first look to hexagonal close-packing and find that packing spherical balloons in a regular tetrahedron  minimizes surface area of our flame-proof system. [Click here](/docs/projects/hcpbabylon) to see simple 3D model (using babylon-js) .



This visual simulation of a module is of great help. I call it a HCP-tetrahedron of 4 balloons, or HCP-tet4 for short. Well, was my intuition correct: do payload calculations agree? I included a new method under my 'dirgible' class that calculates the payload for n balloons surrounded by a regular tetrahedron of flame-proof material. The following plot shows payload vs total volume of hydrogen, and also includes the fact that the balloons are slack at MSL. That is, the flame proof framework is large and allows expansion of inner balloons until alt=8km. 

{% include pload_hcp_v_4bal.html max-width="40%" %}

For reference, I have a chart showing tetrahedra height for volume of H₂. Again keep in mind that the balloons are only filled 40% at MSL. 

{% include ref_tetedge_v_h2vol.html %}

It is clear the advantage in this simple compromise between safety and payload: not every balloon needs to be covered. This is just one example of a realistic approach, but there are many ways to pack spheres and protect them from each other. Different packing methods and architecture can simultaneously provide safety systems and structural integrity to an airship. Roughly speaking, a tetrahedral module with a robust flame protection system (6cm foam)  will lift to 8km and have be 13m in height from base to point. I am estimating the pointy tip can be cut down by ~1m (see red dashes in ref plot), so 12m. Generally, it seems that a the tetrahedron will not surpass a height of 20m for increasing payloads upwards of 6 metric tons! On that front a critical question is how much hydrogen is safe per module? 


Next I will begin to focus on empricial testing of flame-proof systems, as I am convinced of the lift potential of such a craft. 

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

 [hidden link](http://ec2-3-84-53-116.compute-1.amazonaws.com)
