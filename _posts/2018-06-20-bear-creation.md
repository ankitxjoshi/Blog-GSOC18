---
layout:     post
title:      Bear creation with PyFlakesASTBear
date:       2018-06-20
summary:    Lets create a bear with PyFlakesASTBear
categories: blog gsoc
---

<p class="heading">Lets create our first pyflakes bear</p>
<p class="content">
With the <span class='highlight'>PyFlakesASTBear</span> being ready, it's time to use its power and create our
first pyflakes bear. <span class='highlight'>PyFlakesASTBear</span> can be used to write bears that
detect issues in the code base as well as more advanced fixes bears that our
capable of providing a patch to fix them as well.
<p>

<p class="heading">Getting the NoFutureImportBear ready</p>
<p class="content">
Today we will write a simple <span class='highlight'>NoFutureImportBear</span> that will detect
any usage of <span class='highlight'>__future__</span> import and would notify user. This will be a simple
bear that only detects violations.
</p>
<p class="content">
Creating the plugin is quite simple. First create a subclass of coalib
<span class='highlight'>LocalBear</span> and then specify <span class='highlight'>PyFlakesASTBear</span> as a dependency.
</p>
<pre>
from pyflakes_bears.PyFlakesASTBear import PyFlakesASTBear

class NoFutureImportBear(LocalBear):

   BEAR_DEPS = {PyFlakesASTBear}
</pre>
<p class="content">
Now we can use coalib <span class='highlight'>dependency_results.get()</span> method to fetch the names.
After getting the results from metabear we will use <span class='highlight'>get_nodes()</span> method that
accepts a scope and the node type to fetch all the nodes of type <span class='highlight'>FutureImportation</span>.
All the occurence will then be yielded out as violations.
The complete implementation of <span class='highlight'>NoFutureImportBear</span> looks as follows:
</p>
<pre>
class NoFutureImportBear(LocalBear):
  """
  Uses pyflakes-enhance-AST to detect use of future imports
  """
  BEAR_DEPS = {PyFlakesASTBear}

  def run(self, filename, file,
          dependency_results=dict()
          ):
      for result in dependency_results.get(PyFlakesASTBear.name, []):
          for node in result.get_nodes(result.module_scope,
                                       FutureImportation):
              yield Result.from_values(
                  origin=self,
                  message='Future import %s found' % node.name,
                  file=filename,
                  diffs={filename: corrected},
                  line=node.source.lineno)
</pre>

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
