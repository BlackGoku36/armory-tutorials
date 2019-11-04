<div align="center">

# ARMORED-TUT
Tutorials for Armory3d game engine

![GitHub stars](https://img.shields.io/github/stars/BlackGoku36/armory-tutorials?style=flat-square)
![GitHub](https://img.shields.io/github/license/BlackGoku36/armory-tutorials?style=flat-square)

</div>

## Building Docsify Locally
You can do it two way:
1. Doscify-cli
2. Python

after doing it. It should output something like:
```bash
Serving HTTP on 0.0.0.0 port 3000 ...
```
and then open `http://localhost:3000` in your browser.

### Docsify-cli
```bash
# Install docsify-cli
npm install -g docsify-cli
# go to root directory of this tutorial, then
docsify serve
```
Plus side of using docsify cli is that docsify will automatically reload your site for you on saving it.

### Python
```bash
# Make sure you have python, MacOS and Linux have Python 2.7 defaultly installed
# go to root directory of this tutorial, then
python -m SimpleHTTPServer
```

?> **TIP:** If you are using vscode, press **F1** (**fn**+**F1**for macos), select **Tasks: Run Task** and then **docsify**/**python**.

## Snippets

- `codetab`: It will add Tab with haxe code and code explanation dropdown
- `gif`: It will add gif from `docassets`
- `img`: It will add image from `docassets`
- `qa` : It will add dropdown.
