---
layout: default
title: deployments

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

With limitations to static jekyll hosting on gitpages (albeit free, which I am grateful for), I looked elsewhere to begin developing live anaylytical web apps and dashboards. This branch into aws web apps with Flask is a familiar one, but now I intend to bake some serious power into a web app when it comes to demonstrating economic viabiltiy of Supratmos Systems solutions. 

My framework loosely looks like this: Dash/Flask app-> Gunicorn ->Heroku all on an EC2 instance for free.

Getting gunicorn and nginx working  started with a refresher on virtualenv:
'''
virtualenv myenv
cd myenv
. bin/activate
'''



....The path ahead is clear; to address additional traffic and computing needs a Docker image of an app hosted on AWS Fargate container. I am to revisit this link

https://www.chrisvoncsefalvay.com/2019/08/28/deploying-dash-on-amazon-ecs/
