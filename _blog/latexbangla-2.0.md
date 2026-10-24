---
title: "LaTeXbangla 2.0: A Decade Later"
excerpt: "I released LaTeXbangla 0.2 in 2016 to make writing Bangla in LaTeX easier. Ten years later, version 2.0 brings Assamese and LuaLaTeX support, with simpler font setup and the same familiar commands."
collection: blog
redirect_from:
  - /blog/latexbangla-1-0/
date: 2026-10-23
tags:
  - package
  - latex
  - releases
---

![LaTeXbangla: Writing Bangla & Assamese with ordinary LaTeX](/images/latexbangla-logo.png)

[Overleaf demo](https://www.overleaf.com/project/6adbaac1d48d9344680c2fe1/share#6b9d2d41b2de0b80c2de0d11c68ce7c79f33cbe1edef7ec7) \| [Code](https://github.com/Neehan/LaTeXbangla) \| [Manual](https://github.com/Neehan/LaTeXbangla/blob/master/latexbangla/latexbangla.pdf) \| [Bangla example](https://github.com/Neehan/LaTeXbangla/blob/master/examples/bangla.pdf) \| [Assamese example](https://github.com/Neehan/LaTeXbangla/blob/master/examples/assamese.pdf)

In 2016, when I was still in high school, I wrote LaTeXbangla out of frustration. I wanted to LaTeX some math notes in Bangla, but the existing options were limited and I was spending more time configuring fontspec than math. Polyglossia's Bangla support was rough at the time, so I put together a package to handle the common problems. A few years later, I used it to write a [mathematics book](https://www.rokomari.com/book/180324/combinatorics-hatekhori) in Bangla.

Then I mostly left the package alone. To my surprise, it became the standard for typesetting mathematical content in Bangla, with numerous mathematics books published using it. Meanwhile, LaTeX and Polyglossia moved on, and some of my old workarounds stopped working. People [reported](https://tex.stackexchange.com/search?q=latexbangla) problems with numbering, citations, biblatex, and custom font loading, among other things. An update was overdue.

Ten years later, LaTeXbangla is now at version 2.0. These are the changes I care about most:

- LuaLaTeX support alongside XeLaTeX.
- Assamese support, with translated examples and a complete manual.
- Tested defaults for [18 Bangla font families](https://github.com/Neehan/LaTeXbangla/blob/master/examples/supported-fonts.pdf), with adjusted size, spacing, and fallback styles. Other fonts may work too, but have not been tested.
- Custom English and math fonts, including English font changes within a single passage.
- Bangla and Assamese text and digits in equations, along with native citation numbering.
- Native numeric counters and alphabetic list labels for Bangla and Assamese.

The idea is still the same: choose a Bangla font and write ordinary LaTeX. LaTeXbangla takes care of numbering, headings, lists, and references, and switches between Bangla and English fonts as you type.

Overleaf may take a while to include this update. Meanwhile, make a copy of this [demo project](https://www.overleaf.com/project/6adbaac1d48d9344680c2fe1/share#6b9d2d41b2de0b80c2de0d11c68ce7c79f33cbe1edef7ec7) and start writing.

For example, with Kalpurush installed, compile this with XeLaTeX or LuaLaTeX:

```latex
\documentclass{article}
\usepackage{latexbangla}
\setbanglamainfont{Kalpurush}
\setmainfont{TeX Gyre Termes} % Optional: change the English font.

\begin{document}
\section{পিথাগোরাসের উপপাদ্য}
সমকোণী ত্রিভুজের দুই লম্ব বাহুর দৈর্ঘ্য $a$ ও $b$,
এবং অতিভুজের দৈর্ঘ্য $c$ হলে, \textit{Pythagorean theorem} অনুযায়ী,\footnote{সমকোণের বিপরীত বাহুটিই অতিভুজ বা hypotenuse।}
\[a^2+b^2=c^2.\]

A diagonal is the shortest route across a rectangular garden—
বাগানের এক কোণ থেকে বিপরীত কোণে যাওয়ার সরাসরি পথ।
দুই লম্ব বাহুর দৈর্ঘ্য ৩ ও ৪ একক হলে,
\begin{enumerate}
  \item বাহু দুটির দৈর্ঘ্যের বর্গের যোগফল $৩^{২}+৪^{২}=২৫$।
  \item অতিভুজের দৈর্ঘ্য $c=\sqrt{২৫}=৫$ একক।
\end{enumerate}

\textbf{মনে রাখি: a right angle is essential.}
সূত্রটি শুধু সমকোণী ত্রিভুজের জন্য প্রযোজ্য।
\end{document}
```

[![The example rendered with Bangla and English text, equations, a numbered list and a footnote](https://raw.githubusercontent.com/Neehan/LaTeXbangla/master/img/example.png)](https://raw.githubusercontent.com/Neehan/LaTeXbangla/master/img/example.png)

The English text keeps its own font, the section number becomes Bangla, and `\textbf` works even when the Bangla font has no bold face. Typed digits keep their script: `12` stays `12`, and `১২` stays `১২`, in both text and mathematics. You can also choose your math font independently.

For an Assamese document, load `\usepackage[language=assamese]{latexbangla}` and keep the same font commands. Document labels and reference names become Assamese, and alphabetic lists follow the Assamese sequence. You still write ordinary LaTeX; the package takes care of the language-specific details.

If you want different fonts for body text, sans-serif text, and typewriter text, you can choose those separately for each script. Even a different English font used for just one quotation stays in place when Bangla or Assamese appears in the passage.

The old numbering and citation problems are fixed, and more of the language support now comes from Polyglossia. The [manual](https://github.com/Neehan/LaTeXbangla/blob/master/latexbangla/latexbangla.pdf) is available in English, Bangla, and Assamese, with code beside its rendered output so you can see what each example does.

If you used the old version, the manual has a migration section to help you update your documents. If you are starting a new one, try the [example](https://github.com/Neehan/LaTeXbangla/tree/master/examples).

The book I mentioned earlier also left me with a book template. I’ve now cleaned it up and released it as [BOOKbangla](https://github.com/Neehan/BOOKbangla), a companion class for Bangla and Assamese books built on memoir and LaTeXbangla. It gives you chapter designs, worked examples and exercises with hints, while keeping memoir’s customization options available. The [example book](https://github.com/Neehan/BOOKbangla/blob/master/examples/bangla/main.pdf) includes a full chapter from *কম্বিনেটরিকসে হাতেখড়ি*. You can make a copy of the [BOOKbangla Overleaf demo](https://www.overleaf.com/project/6adc6a64e207c9e858a5c4ba/share#f620335030ef2a824fafcc0cfb1f6e2548f3d2154965545f) to start your own book.

Thanks for Reading!
