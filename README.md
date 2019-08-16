# armory-tutorials

- [Game save & load mechanism](Save_Load.md)

<button class="btn js-toggle-dark-mode">LightMode/DarkMode</button>

<script>
const toggleDarkMode = document.querySelector('.js-toggle-dark-mode');
const cssFile = document.querySelector('[rel="stylesheet"]');
const originalCssRef = cssFile.getAttribute('href');
const darkModeCssRef = originalCssRef.replace('just-the-docs.css', 'dark-mode-preview.css');

jtd.addEvent(toggleDarkMode, 'click', function(){
  if (cssFile.getAttribute('href') === originalCssRef) {
    cssFile.setAttribute('href', darkModeCssRef);
  } else {
    cssFile.setAttribute('href', originalCssRef);
  }
})
</script>
