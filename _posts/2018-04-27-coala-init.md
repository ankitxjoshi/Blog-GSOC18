---
layout:     post
title:      GSoC'18 coala Init
date:       2018-04-27
summary:    The much awaited results of Google Summer Of Code are out
categories: blog gsoc
---

<p class="heading">What is GSoC ?</p>
<p class="content">
GSoC is an annual program administered and funded by Google for students all around the world to contribute to open source software in summer. You may think that only undergrads are allowed to take part in this program, but that’s not the case. Undergrads, students in graduate programs, PhD candidates can take part in GSoC. As computer science or software engineering students GSoC one of the most prestigious things that students can accomplish.
<p>

<p class="heading">What is open source ?</p>
<p class="content">
Open source software is the heart and soul of many organizations. It would be real hard to imagine more secure and reliable software without open source initiatives. Everyday hundreds and thousands of developers contribute to different open source software. Open source projects, release the source code to the public so that anyone can study it, see how it works, and change and redistribute it if they want to. </p>

<p class="heading">Why coala (with a lowercase c) ?</p>
<p class="content">
coala provides a unified interface for linting and fixing code with a single configuration file, regardless of the programming languages used. You can use coala from within your favorite editor, integrate it with your CI, get the results as JSON, or customize it to your needs with its flexible configuration syntax.

coala has a set of official bears (plugins) for several languages, including popular languages such as C/C++, Python, JavaScript, CSS, Java and many more, in addition to some generic language independent algorithms.
</p>

<p class="heading">About my project</p>
<p class="content">
The idea, here, is to integrate <a class="hyperlink" href="https://github.com/PyCQA/pyflakes"> pyflakes-enhanced AST</a> into coala as a metabear which can then be used to develop various plugins. The second part of the project involves redesigning flake8 plugins flake8-future-import and flake8-builtins in such a way that they use pyflakes-enhanced AST over python AST. Finally, a wrapper is to be created which supplies a python AST to flake8 plugins so that they work as it is.
</p>
<p class="content">
This would allow coala developers to utilise the full potential of enhanced AST rather than simply depending on python AST and so, a whole lot of rework to do the basic traversing and collection of important nodes gets saved. Pyflakes provides with a basic API that does the traversing. So, if a developer uses enhanced AST he just needs to work on the implementation of the new logic that their plugin provides and not about the fidelity of the basic node handlers.
</p>

<style>
.heading {
    color:#c62828;
    font-size:1.5em;
}

.content {
    text-align:justify;
    background: #ffffff;
}

a.hyperlink {
    color:#b71c1c;
    text-decoration-color: red !important;
}

a.hyperlink:hover, a.hyperlink:active {
    color:#c51162;
    text-decoration-color: red !important;
}
</style>
