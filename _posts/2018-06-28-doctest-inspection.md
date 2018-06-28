---
layout:     post
title:      Inspecting code within code
date:       2018-06-28
summary:    Introducing static code analysis of python doctest
categories: blog gsoc
---

<p class="heading">What are doctest?</p>
<p class="content">
The doctest module is often considered easier to use than the unittest,
though the later is more suitable for more complex tests. doctest is a test
framework that comes prepackaged with Python. The doctest module searches
for pieces of text that look like interactive Python sessions inside of the
documentation parts of a module, and then executes (or reexecutes) the
commands of those sessions to verify that they work exactly as shown,
i.e. that the same results can be achieved. In other words: The help text of
the module is parsed, for example, python sessions. These examples are
run and the results are compared against the expected value.
<p>

<p class="heading">Aren't their tools for analysing doctests?</p>
<p class="content">
Python doctest are stored as plain strings attached to the body of every
<a href="https://greentreesnakes.readthedocs.io/en/latest/nodes.html#FunctionDef">FunctionDef</a>
node. Thus aren't readily available as cpython nodes for analysis and thus are
often left out while detecting code-smells and antipatterns in the code. But doctest
are after all python code and might vary from simple command line instruction to
complex code. This begs for the need to analyze them too.
<p>

<p class="heading">Can pyflakes AST do this job?</p>
<p class="content">
Yes!!! As we have already discussed about <span class="highlight">pyflakes scopes</span>
while creating the NoFutureImportBear, one interesting scope that pyflakes provides is
<span class="highlight">DoctestScope</span>. Consider <span class="highlight">DoctestScope</span>
as a <span class="highlight">ModuleScope</span> generated for every doctest. In simple
words <span class="highlight">DoctestScope</span> provides analysis of every
doctest present as individual python programs.
<p>

<p class="heading">How different is DoctestScope from ModuleScope?</p>
<p class="content">
Not at all different. This is the main adavantage that pyflakes provides us.
Any analysis done for a module using pyflakes can also be done for doctests
present in that module. Thus all plugins can magically also analyze doctest and
thus now the code analysis is no longer limited to only module but also to the
doctest present in them. In the next post, we will discuss a coala bear
that leverages this magic to check for pep8 naming convention.
<p>


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

pre {
  font-size:0.9em;
}

.highlight {
  background: #fafafa;
  color: black;
  font-family: Optima, sans-serif;
}
</style>
