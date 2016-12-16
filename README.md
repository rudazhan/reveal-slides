# Web Presentations with reveal.js

Web presentation using `reveal.js` and `remark.js`, learning from @velvia.

[reveal.js Documentation](https://github.com/hakimel/reveal.js)

Starting Steps:

1. make sure you have `node.js` and `grunt`.
  - (global) update is recommended: `npm update`
1. download the latest `reveal.js` distribution and install here.
  - `tar -C . -zxf ~/Downloads/reveal*{.tar.gz,.zip}`
  - `npm install`
1. Serve the presentation on localhost:8000
  - `grunt serve`

Export to PDF: (Chrome)

1. Include `print-pdf` in URL query string
1. Open the in-browser print dialog:
  - Destination: Save as PDF
  - Layout: Landscape
  - Margins: None
  - Background graphics: true
1. Save

## The reveal.js Repository

Project Structure: (in Git)

    css/      Core reveal.js styles
    js/       Core reveal.js JavaScripts
    plugin/   Extensions to reveal.js
    lib/      All other third party assets (js/, css/, font/)
    test/     Test files of reveal.js

### Plugins

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

Markdown:

1. `marked` for GitHub Flavored Markdown.
1. Data path is specified with section element's `data-markdown` attribute.
1. Section partition could be customarily defined with `data-separator` and `data-separator-vertical` attributes by RegEx.
1. HTML hacks
  - newline `&nbsp;`
  - paragraph `<p>`
  - add attribute to current section element, like
  `<!-- .slide: data-background="img.png" -->`
  - align image (e.g. to left/right) with attribute `align` of `img` element.

Speaker note:

1. Prepend `NOTE: ` before your speaker note, and put at end of section.

### Third Party Assets

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


## Features

- format support:
  - Markdown: heading, quote, list, table, image, link (support internal reference)
  - MathJax: math
  - code syntax highlighting with `highlight.js` and scrollbar
  - PDF export
- mobile support
- speaker notes: `NOTE: ` at end of section.

Key binding:
- navigation: space, arrow keys.
- overview (toggle): ESC
- speaker view: `s`
- full screen: `f`
- blackout: `b` or `.`
- zoom: Alt+click (doesn't solve content overflow)

- fragments: step through, styles (sizes, fade, highlight)
- transition styles: `?transition=` none, fade, slide; convex, concave; zoom;
- themes: Black (default), White, League, Sky, Beige, Simple, Serif, Blood, Night, Moon, Solarized
- slide background:
  - color/image: `data-background=""` with CSS color or path (GIF supported).
  - image tiling: `data-background-repeat="repeat"`
  - video: `data-background-video="video.mp4,video.webm"`
  - transition: `data-background-transition=""`
- advanced JavaScript support

Footnote is currently not supported.
You may hack one with `<span class="footnote">Footnote text here.</span>` at end of section.


## Demos

Example slides using `reveal.js`:

- [OSGeo-live 8.5 Example](http://live.osgeo.org/en/presentation/index.html#/)
- [Eric Theise - Let's Talk About Your Geostack](http://erictheise.github.io/geostack-deck/)


## Design

Beware of content overflow (at bottom), which depends on the screen.
reveal.js does not check content overflow.
Mind that office projectors are typically square;
modern projectors tend to be wide;
computer displays are wide;
mobile screens are wider.

Slide background color/image could be used for chapter transitioning.

Typically you wouldn't want uppercased headings, overwrite theme CSS parameter on `text-transform` to `none`.

The epiphany of good presentation:
To pack information, use visualizations (diagram, image, gif, video).
