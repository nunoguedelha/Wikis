# Latex to Markdown: Rendering Latex in Github

### Wikipedia with MathJax (Chrome extesion)

repo by bgromov: https://github.com/bgromov/wiki-mathjax

### Github mathjax

#### Chrome extension by **orsharir**

The link to the repo ttps://github.com/orsharir/github-mathjax

#### Firefox extension by **Silvio Traversaro**

tobiasdiez commented on Sep 28, 2017:
> I just tried it with firefox (cloned the repository and invoked web-ext run). The add-on runs and works perfectly without any modifications! It would be really awesome if you could upload it to https://addons.mozilla.org/.


Silvio commented:
https://github.com/orsharir/github-mathjax/issues/6#issuecomment-371653238

the extension submitted by Silvio was removed after review on March 16th 2018.

Repo: https://github.com/traversaro/github-mathjax-firefox

> Apparently you can also easily distribute the extension on your own as .xpi file, without passing through the addons.mozilla.org website, but just uploading the extension to https://addons.mozilla.org/en-US/developers/addon/submit/distribution for signing it.
> See https://github.com/traversaro/github-mathjax-firefox for the version of this extension for Firefox.

Just click on the download link while browsing with Firefox, and Firefox install automatically the `.xpi` file:
https://github.com/traversaro/github-mathjax-firefox/releases/download/v0.2.3/github_with_mathjax-0.2.3.xpi

### Installing a add-on on Firefox

(How-to from add-on examples https://github.com/mdn/webextensions-examples.git)

There are a couple ways to try out the example extensions in this repository.

1. Open Firefox and load `about:debugging` in the URL bar. Click the
   [Load Temporary Add-on](https://developer.mozilla.org/en-US/Add-ons/WebExtensions/Temporary_Installation_in_Firefox)
   button and select the `manifest.json` file within the
   directory of an example extension you'd like to install.
   Here is a [video](https://www.youtube.com/watch?v=cer9EUKegG4)
   that demonstrates how to do this.
2. Install the
   [web-ext](https://developer.mozilla.org/en-US/Add-ons/WebExtensions/Getting_started_with_web-ext)
   tool, change into the directory of the example extension
   you'd like to install, and type `web-ext run`. This will launch Firefox and
   install the extension automatically. This tool gives you some
   additional development features such as
   [automatic reloading](https://developer.mozilla.org/en-US/Add-ons/WebExtensions/Getting_started_with_web-ext#Automatic_extension_reloading).


Extension repo: /Users/nunoguedelha/tools/github-mathjax
Examples repo:  /Users/nunoguedelha/tools/webextensions-examples
