# Reveal.js

[TOC]

## 1. Prepare

- Node.js
- Reveal.js: `git clone git@github.com:hakimel/reveal.js.git`
    - Install
        - `cd reveal.js/`
        - `npm install node-sass puppeteer`
        - `npm install`
        - `npm audit fix`
- Local MathJax
    - `cd plugin/; git clone git@github.com:mathjax/MathJax.git`

## 2. Configeration

- Math: `{ src: 'plugin/math/math.js', async: true }`
- MathJax

```javascript
math: {
    mathjax: 'plugin/MathJax/MathJax.js',
    config: 'TeX-AMS_HTML-full'  // See http://docs.mathjax.org/en/latest/config-files.html
    // pass other options into `MathJax.Hub.Config()`
    // TeX: { Macros: macros }
}
```

- Max Scale: `maxScale: 1.5`

## 3. Meta Data

- Title: `<title><Title></title>`

## 4. Slide

- Normal Slide: `<html> -> <body> -> <div class="reveal"> -> <div class="reveal"> -> <section>`

```xml
<section>
    ...
</section>
```

- Vertical Slide: `<html> -> <body> -> <div class="reveal"> -> <div class="reveal"> -> <section> -> <section>`

```xml
<section>
    <section>
        ...
    </section>
</section>
```

### 4.1. Content

- Text: `<p>`

```xml
<p data-markdown>
    ...
</p>
```

- Picture

```xml
![<Title>](<PicDir>)

<img src="<PicDir>" width="<Width>" height="<Height>"/>
```

- Video: `<video>`

```xml
<video controls>
    <source src="<VideoDir>" type="video/mp4">
</video>
```

- Aside: `<aside>`

```xml
<aside data-markdown class="notes">
    ...
</aside>
```

#### 4.1.1. Markdown

- `data-markdown`

### 4.2. External Markdown File

- Working Directory: Reveal.js root dir
- Can use all html tag

```xml
<section data-markdown="asset/md/slide.md"
		 data-separator="^\r?\n====\r?\n$"
		 data-separator-vertical="^\r?\n----\r?\n$"
		 data-separator-notes="^\r?\n[Nn]otes?:\r?\n$">
</section>
```

```Markdown
...

Note:

...

====  # Herizon Slide Seperate

...

Note:

...

----  # Vertical Slide Seperate

...

Note:

...
```

## 5. Run

- `npm start`
    - Run indicating port: `npm start -- --port=<Port>`
    - Default port: 8000
- Open `http://localhost:<Port>` to view slides
- Full Screen: `f`
    - Exit: `<ESC>`
- Presentation Mode: `s`
- Exit: `<CTRL-c>`

## 6. Distribute

- Export to PDF: `http://localhost:<Port>/?print-pdf`
- Export with speaker note: `http://localhost:<Port>/?print-pdf&showNotes=true`
- Print
