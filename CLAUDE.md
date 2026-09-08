# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Stack & Conventions

This project has two hard constraints that must never be violated:

1. **Single-file project.** The entire project must live in one `index.html` file, with all CSS and JavaScript inlined using `<style>` and `<script>` tags. Do not split markup, styles, or scripts into separate files, and do not add additional HTML pages. Linking to external images and to external CSS/JavaScript libraries (e.g. via CDN `<link>`/`<script src>` tags) is allowed. This constraint exists so the finished project can be copy-pasted as a single file for sharing in class and on single-file code platforms (e.g. CodePen, JSFiddle).
2. **Vanilla only, no build step.** Use plain HTML, CSS, and JavaScript only. No frameworks or libraries that require a build/compile step (e.g. React, Vue, Angular, JSX, TypeScript, Sass/Less, bundlers like Webpack/Vite). The file must run directly by opening it in a browser, with no installation or build process required.
