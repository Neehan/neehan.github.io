---
title: "LaTeXbangla 1.0: Ten Years Later"
excerpt: "I released LaTeXbangla 0.2 in 2016 to make writing Bangla in LaTeX easier. Ten years later, version 1.0 brings it up to date with simpler font setup and the same familiar commands."
collection: blog
date: 2026-10-23
tags:
  - package
  - latex
  - releases
---

![LaTeXbangla: Writing Bangla with ordinary LaTeX](/images/latexbangla-logo.png)

[Code](https://github.com/Neehan/LaTeXbangla) \| [Manual](https://github.com/Neehan/LaTeXbangla/blob/master/latexbangla/latexbangla.pdf) \| [Example](https://github.com/Neehan/LaTeXbangla/blob/master/examples/example.pdf)

In 2016, when I was still in high school, I wrote LaTeXbangla out of frustration. I wanted to typeset some math notes in Bangla, but the existing options were limited. I was spending more time configuring fonts than writing the notes. Polyglossia's Bangla support was rough at the time, so I put together a package to handle the common problems. A few years later, I used it to write a [mathematics book](https://www.rokomari.com/book/180324/combinatorics-hatekhori) in Bangla.

Then I mostly left the package alone. To my surprise, it became the standard for typesetting mathematical content in Bangla, with numerous mathematics books published using it. Meanwhile, LaTeX and Polyglossia moved on, and some of my old workarounds stopped working. People [reported](https://tex.stackexchange.com/questions/99606/how-to-write-bengali-in-latex/335214#335214) problems with numbering and citations. An update was overdue.

Today, ten years later, I am releasing version 1.0 of LaTeXbangla on GitHub, with a CTAN update to follow. The idea is still the same: choose a Bangla font, then write ordinary LaTeX. A font lets you display Bangla characters, but a Bangla document also needs numbering, headings, lists, and references to work together. LaTeXbangla handles that setup and switches between Bangla and English fonts as you type.

For example, with Kalpurush installed, compile this with XeLaTeX:

```latex
\documentclass{article}
\usepackage{latexbangla}
\setbanglamainfont{Kalpurush}
\setmainfont{TeX Gyre Termes} % Optional: change the English font.

\begin{document}
\section{পিথাগোরাসের উপপাদ্য}
The Pythagorean theorem থেকে আমরা পাই,
\[৩^২+৪^২=২৫.\]
\textbf{অতিভুজের দৈর্ঘ্য ৫ একক।}
\end{document}
```

The English text keeps its own font, the section number becomes Bangla, and `\textbf` works even when the Bangla font has no bold face. Typed digits keep their script: `12` stays `12`, and `১২` stays `১২`, in both text and mathematics. You can also choose your math font independently.

This version requires only one Bangla font. I tested Kalpurush, SolaimanLipi, Siyam Rupali, Noto Serif Bengali, Noto Sans Bengali, and FreeSerif, including bold and italic text with real faces or synthetic fallbacks. I also fixed the numbering and citation compatibility issues and now rely on Polyglossia for more of the language support. The [manual](https://github.com/Neehan/LaTeXbangla/blob/master/latexbangla/latexbangla.pdf) is available in both English and Bangla, with code beside its rendered output.

If you used the old version, the manual has a migration section to help you update your documents. If you are starting a new one, try the [example](https://github.com/Neehan/LaTeXbangla/tree/master/examples). The goal is the same as it was in high school: spend less time setting up LaTeX and more time writing Bangla.
