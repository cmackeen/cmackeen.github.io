---
layout: default
title: iXAFS auto analysis
nav_order: 3
permalink docs/exafs
---

# Learning from 25 Years of Processed EXAFS Data

s the spirit of this software project i 

## Search enabled

```yaml
# Enable or disable the site search
search_enabled: true
```

## Aux links

```yaml
# Aux links for the upper right navigation
aux_links:
    "CV quick link":
      - "yada--yada-yadaddurlackeen"
```

## Color scheme

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

See [Customization]({{ site.baseurl }}{% link docs/customization.md %}) for more information.

{: .fs-6 .fw-300 }
