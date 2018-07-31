# Web Presentations with reveal.js

Web presentation using `reveal.js`, learning from [Evan Chan](https://github.com/velvia).

[reveal.js Documentation](https://github.com/hakimel/reveal.js)

Example slides using `reveal.js`:

- [OSGeo-live 8.5 Example](http://live.osgeo.org/en/presentation/index.html#/)
- [Eric Theise - Let's Talk About Your Geostack](http://erictheise.github.io/geostack-deck/)


## Fast User Guide

Starting Steps:

1. make sure you have Node.js `nodejs` and `grunt` installed.
  - Node package update is recommended: `npm update`
1. download the latest `reveal.js` distribution and install here.
  - `tar -C . -zxf ~/Downloads/reveal*{.tar.gz,.zip}`
  - `npm install`
1. Serve the presentation locally.
  - `grunt serve`

New Slides Deck:

1. create a directory (kebab-case) at root for new deck;
1. copy `index.html` from a recent deck, modify `<title>`, optionally change `data-markdown`, themes/stylesheet and configuration script section;
1. write slides content in `content.md`.
1. Serve this project with endpoint being the directory name of the new deck: `localhost:8000/<deck-name>/?<parameter-list>#/major/minor`.

Presentation Key Binding:

- `?`, list keyboard shortcuts;
- `f`, full screen;
- `s`, speaker view;
- `b` or `.`, blackout;
- navigation: `n`/`p`, `j`/`k`/`h`/`l`, space and arrow keys, `home`/`end`;
- `o` or `ESC`: toggle overview;
- Alt+click: zoom (Does not solve content overflow.)
- `m`: menu (plugin enabled)

Export to PDF: (Chrome)

1. Include `print-pdf` in URL query string
1. Open the in-browser print dialog:
  - Destination: Save as PDF
  - Layout: Landscape
  - Margins: None
  - Background graphics: true
1. Save


## Project Structure

- slides deck directories (kebab-case): `index.html`, `content.md`, images and affliate files.
- `lib/`: custom assets on reveal.js
    - `js/`, local copy of MathJax (git-ignored).
    - `css/`, custom styles.
    - `plugin/`: third-party extensions to reveal.js.
- `reveal.js-<version>`, local copy of reveal.js (git-ignored).
    - `js/`: Core reveal.js JavaScripts
    - `css/`: Core reveal.js styles
    - `test/`: Test files of reveal.js
    - `plugin/`: Extensions to reveal.js
    - `lib/`: All other third party assets (js/, css/, font/)
- `node_modules`: node.js modules (git-ignored) for reveal.js.


## Features

- [Basics](#basics) with Markdown: headings, italics/bold/quote, link (support internal reference), list, table, image
- code syntax highlighting with `highlight.js` and scrollbar
- [Math](#math) with MathJax: math symbol and equation
- [UML diagrams](#uml-diagrams) with Mermaid
- [Speaker notes](#speaker-notes)
- mobile support: second screen
- Multiplexing (master-client presentation broadcasting)

Others:

- themes: Black (default), White, League, Sky, Beige, Simple, Serif, Blood, Night, Moon, Solarized
- graphic background:
  - color/image: `data-background=""` with CSS color or path (GIF supported).
  - image tiling: `data-background-repeat="repeat"`
  - video: `data-background-video="video.mp4,video.webm"`
  - iframe
  - transition: `data-background-transition=""`
- slide fragments: step through, styles (change sizes, fade, highlight)
- advanced JavaScript support

Othe plugins:

- [slideout menu](https://github.com/denehyg/reveal.js-menu)
- anything: chart.js, D3.js
- chalkboard
- [pug](https://github.com/pugjs/pug): a template engine for Node.js.

URL parameters (overriding):

- transition styles: `?transition=` none, fade, slide; convex, concave; zoom;

Footnote is currently not supported.
You may hack one with `<span class="footnote">Footnote text here.</span>` at end of section.

### Basics



Markdown plugin configuration:

1. use `marked` for GitHub Flavored Markdown.
1. Data path is specified with section element's `data-markdown` attribute.
1. Section partition could be customarily defined with `data-separator` and `data-separator-vertical` attributes by RegEx.

HTML hacks:

- newline `&nbsp;`
- paragraph `<p>`
- HTML element attributes:
    - slide: `<!-- .slide: data-background="img.png" -->`;
    - fragment: `<!-- .element: class="fragment" data-fragment-index="1" -->`;
- align image (e.g. to left/right) with attribute `align` of `img` element.

### Math

MathJax:

- Install MathJax for offline localhost rendering.
  - `wget https://github.com/mathjax/MathJax/archive/2.7.0.zip`
  - `unzip MathJax*.zip -d ./lib/js && rm $_`
- Provide path to MathJax repository in `Reveal.initialize()`
  ```js
  math: {
      mathjax: '/lib/js/MathJax-2.6/MathJax.js',
      config: 'TeX-AMS_HTML-full'
  }
  ```
- Call the math wrapper plugin in dependencies (add comma after the item before)
  - `{ src: '/plugin/math/math.js', async: true }`
- Math syntax supported in markdown
  - Markdown inline code quotes + LaTeX
  - Gollum type: `\\(\\)` for inline math, `\\[\\]` for display math.

### Speaker notes

Prepend `NOTE: ` before your speaker note, and put at end of section.
One paragraph only.

### UML diagrams

Mermaid:

1. mermaid.js
  - `wget https://cdn.rawgit.com/knsv/mermaid/0.5.7/dist/mermaid.min.js -P ~/repo/reveal.js/lib/js`
  - Load JS, `<script src="../lib/js/mermaid.min.js"></script>`
  - Load CSS, `<link rel="stylesheet" href="../lib/css/mermaid.css">`
  - Config diagram rendering with `mermaid.initialize()`
  - Mermaid [messes inline css](knsv/mermaid#157), this is fixed with interface `cloneCssStyles:false` in [0.5.0](157#issuecomment-109765512).
  - You may consider use [a mermaid plugin](https://github.com/ludwick/reveal.js-mermaid-plugin) for reveal.js.
1. [Mermaid CLI](http://knsv.github.io/mermaid/mermaidCLI.html)
  - Mermaid depends on PhantomJS (version ^1.9.0)
    - Global installation: `npm install -g phantomjs@^1.9.0`
    - Local installation: `npm install phantomjs@^1.9.0`
  - Install Mermaid
    - Global installation: `npm install -g mermaid`
    - Local installation: `npm install mermaid --save-dev`
  - Compile `.mermaid` source files to PNGs:
    - `mermaid *.mermaid`
    - `mermaid -e ../node_modules/phantomjs/bin/phantomjs *.mermaid`, assume local installation of PhantomJS.

[Yeoman generator for Reveal.js](https://github.com/slara/generator-reveal)

