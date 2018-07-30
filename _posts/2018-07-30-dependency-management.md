---
layout:     post
title:      Dependency Management in Python
date:       2018-07-30
summary:    Typical way of managing project dependency today
categories: blog gsoc
---

<p class="heading">What is Dependency Management?</p>
<p class="content">
Software projects rarely work in isolation. In most cases, a project
relies on reusable functionality in the called libraries. Dependency management
helps manage all the libraries required to make an application work. It make
sure that a package installed works in similar way across every environment.
Note package in this context is being used as a synonym for a distribution i.e
a bundle of software.
<p>

<p class="heading">The pythonic way to install dependency</p>
<p class="content">
<span class='highlight'>pip</span> is the most used package manager for python.
It's acronym for <span class='highlight'>pip installs package</span>.
<span class='highlight'>easy_install</span> is also an archaic way to install
dependency. pip allows to install, upgrade, list and uninstall dependencies
easily in python.
<p>
<p class="content">
The basic way of installing dependencies using pip is
<pre> pip install &lt;package&gt; </pre>.
</p>

<p class="heading">Always keep a requirements.txt</p>
<p class="content">
As long as the library is only being used by you, you don't require such
sophistication but if you're sharing a project with others,
you need to specify the external packages that the project requires.
The recommended approach is to use a requirements.txt file that
contains a list of commands for pip that installs the required versions
of dependent packages. However note that pip does not use requirements.txt when
your project is installed as a dependency by others. The dependecies are simply
stated along with their version in form of plain text.
</p>
<pre>
coala==0.1
coala-bears==0.1
coala-pyflakes==0.1
</pre>

<p class="heading">Maintaining a setup.py</p>
<p class="content">
This is another type of dependency specification for Python packages.
Setup.py is a standard for distributing and installing Python libraries.
It correctly install both the package as well as additional dependencies
for the package.
</p>
<p class="content">
<span class=".highlight">install_requires</span> is looked by pip to fetch the list of desired
dependencies.
<pre>
setup(
    install_requires=[
        "coala-pyflakes>=0.1"
    ],
)
</pre>
</p>

<p class="heading">Installing dependency not on PyPI</p>
<p class="content">
You can specify dependencies for your Python project in setup.py by referencing
packages on the Python Package Index (PyPI). But what if you want to depend on
some package that hasn't been released or rests inside your private repository.
Using dependency_links, you can reference a package's source repository directly.
</p>
<pre>
setup(
    install_requires=[
        "coala-pyflakes"
    ],
    dependency_links=[
        "git+https://gitlab.com/macbox7/coala-pyflakes#egg=coala-pyflakes"
    ]
)
</pre>
Any particular commit or version can be also specified by appending
<span class=".highlight">@</span> after the URL. This provides a more specific
control on the library.
<pre>
setup(
    install_requires=[
        "coala-pyflakes==0.1"
    ],
    dependency_links=[
        "git+https://gitlab.com/macbox7/coala-pyflakes@0.1#egg=coala-pyflakes"
    ]
)
</pre>

<p class="heading">Deploying package on PyPI</p>
<p class="content">
By default, pip will install packages from the python.org pypi server.
If you work at a place with proprietary code, you may wish to run
your own PyPI server. This will allow you to install your own packages
as easily as those from the main PyPI server.
</p>
<p class="content">
It’s actually much easier to set this up than you might think: your
PyPI server can be as simple as an HTTP server serving a folder that tarballs
of your Python project!
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

pre {
  font-size:0.9em;
}

.highlight {
  color: #c62828;
  font-family: Optima, sans-serif;
}
</style>
