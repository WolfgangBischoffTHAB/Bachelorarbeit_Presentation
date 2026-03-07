https://www.youtube.com/watch?v=a6ioNtv2H-E - part 1
https://www.youtube.com/watch?v=squd3Kh8iWs - part 2
https://www.youtube.com/watch?v=O2qUL6nJFBM - part 3

Install node js: https://nodejs.org/en/download

cd C:\Users\lapto\dev
mkdir js
cd js
git clone https://github.com/hakimel/reveal.js.git
cd reveal.js && npm install

# Präsentations Server starten

cd C:\Users\lapto\dev\Bachelorarbeit_Presentation
npm start

visit http://localhost:8000

F11 to enter fullscreen mode.
ESC to exit fullscreen mode.


# Markdown

https://revealjs.com/markdown/

```
<section data-markdown>
  <textarea data-template>
    ## Slide 1
    A paragraph with some text and a [link](https://hakim.se).
    ---
    ## Slide 2
    ---
    ## Slide 3
  </textarea>
</section>
```

# Font Family

# Background Color

In index.html

```
<link rel="stylesheet" href="dist/theme/black.css">
```

# Add Logo




# Export to PDF

cd C:\Users\lapto\dev\js\reveal.js
npm install -g decktape

decktape automatic http://localhost:8000/ bthesis_presentation_wolfgangbischoff_2228538.pdf


<p align="center">
  <a href="https://revealjs.com">
  <img src="https://hakim-static.s3.amazonaws.com/reveal-js/logo/v1/reveal-black-text-sticker.png" alt="reveal.js" width="500">
  </a>
  <br><br>
  <a href="https://github.com/hakimel/reveal.js/actions"><img src="https://github.com/hakimel/reveal.js/workflows/tests/badge.svg"></a>
  <a href="https://slides.com/"><img src="https://static.slid.es/images/slides-github-banner-320x40.png?1" alt="Slides" width="160" height="20"></a>
</p>

reveal.js is an open source HTML presentation framework. It enables anyone with a web browser to create beautiful presentations for free. Check out the live demo at [revealjs.com](https://revealjs.com/).

The framework comes with a powerful feature set including [nested slides](https://revealjs.com/vertical-slides/), [Markdown support](https://revealjs.com/markdown/), [Auto-Animate](https://revealjs.com/auto-animate/), [PDF export](https://revealjs.com/pdf-export/), [speaker notes](https://revealjs.com/speaker-view/), [LaTeX typesetting](https://revealjs.com/math/), [syntax highlighted code](https://revealjs.com/code/) and an [extensive API](https://revealjs.com/api/).

---

Want to create reveal.js presentation in a graphical editor? Try <https://slides.com>. It's made by the same people behind reveal.js.

---

### Getting started
- 🚀 [Install reveal.js](https://revealjs.com/installation)
- 👀 [View the demo presentation](https://revealjs.com/demo)
- 📖 [Read the documentation](https://revealjs.com/markup/)
- 🖌 [Try the visual editor for reveal.js at Slides.com](https://slides.com/)
- 🎬 [Watch the reveal.js video course (paid)](https://revealjs.com/course)

---
<div align="center">
  MIT licensed | Copyright © 2011-2026 Hakim El Hattab, https://hakim.se
</div>
