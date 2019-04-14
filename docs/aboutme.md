---
layout: default
title: AboutCam
nav_order: 4
---

# Customization
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## Professional
{: .d-inline-block }

Diverse experience
{: .text-delta }

Over my academic career I have undertaken a variety of projects, most likely a consequence of my curiousity. I enjoy cycles of working passionately on a project of interest, and I am result driven. I particularly enjoy programming projects; I have been building tools and analysis frameworks to promote thorough quantitative research while cutting out brute force tedium (a common grad student experience).

#### Example
{: .no_toc }

```yaml
# Color scheme currently only supports "dark" or nil (default)
color_scheme: "dark"
```
<button class="btn js-toggle-dark-mode">Preview dark color scheme</button>

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

## Specific visual customization

To customize your site’s aesthetic, open `_sass/custom/custom.scss` in your editor to see if there is a variable that you can override. Most styles like fonts, colors, spacing, etc. are derived from these variables. To override a specific variable, uncomment its line and change its value.

For example, to change the link color from the purple default to blue, open `_sass/custom/custom.css` and find the `$link-color` variable on line `50`. Uncomment it, and change its value to our `$blue-000` variable, or another shade of your choosing.

#### Example
{: .no_toc }

```scss
// ...
//
// $body-text-color: $grey-dk-100;
// $body-heading-color: $grey-dk-300;
$link-color: $blue-000;
//
// ...
```

_Note:_ Editing the variables directly in `_sass/support/variables.scss` is not recommended and can cause other dependencies to fail.

---
