---
layout:     post
title:      PyFlakes Enhanced AST
date:       2018-04-27
summary:    Making plugins simpler and powerful with PyFlakes AST
categories: blog gsoc
---

<p class="heading">The Abstract Syntax Tree</p>
<p class="content">
In programming, abstract syntax trees are data structures (trees to be exact) that represents the syntactical structure of the source code. They differ from concrete syntax trees as the resulting data structure is relatively abstracted (ie punctuation and parentheses are excluded). By parsing source code into an AST, it is much easier to perform static analysis of the code.
<p>

<p class="heading">The traditional plugin development approach</p>
<p class="content">
Flake8 is the most commonly used linter framework when it comes to static code analysis for python. It wraps around the tools pyflakes, pycodestyle and mccabe scripts. These tools are implemented on different interfaces and flake8 combines their potential.
</p>
<p class="content">
There has not been an option to use the pyflakes-enhanced-AST based checker API to create custom linters. Thus, the full potential of enhanced AST isn't utilized , a whole lot of rework is required to do the basic traversing and collection of important nodes.
</p>

<p class="heading">Plugins utilizing PyFlakes AST</p>
<p class="content">
Pyflakes provides with a basic API that does the traversing. So, if a developer uses enhanced AST he just needs to work on the implementation of the new logic that his/her plugin provides and not about the fidelity of the basic node handlers.
</p>
<p class="content">
The reason for choosing pyflakes over its equivalents like Astroid lies in its simplicity and provision of lots of re-usable analysis. Astroid is more detailed and powerful, thus making it complicated. Plus, there is a much larger ecosystem of flake8 plugins.
</p>

<p class="heading">When coala meets pyflakes: PyFlakesASTBear</p>
<p class="content">
PyFlakesASTBear is a metabear that will supply pyflakes-enhance-AST to other dependent bears. These
bears will then utilise the full extent of AST to easily perform static analysis on code.
</p>
<p class="content">
Pyflakes provides a Checker class that can be extended to retrieve the enhanced AST. This will be done by PyFlakesASTBear
class whose task would be to initialize Checker class with an abstract syntax tree (_ast.Module) and a file name.
</p>
<p class="content">
The current implementation of the Checker class persists the information about the visited scopes in the deadScopes member variable.
Pyflakes maintains scope information at 5 levels they are ModuleScope(Module level view), ClassScope(Class level view), FunctionScope(Function level view), GeneratorScope(Generator level view) and finally DoctestScope.
</p>
<p class="content">
The scopes keep a record of all bindings that have been used in the program.
Bindings are basically Classes, Functions, Variables, Arguments that have been used in the program.
</p>
<p class="content">
The pseudo code for the PyFlakesASTBear is:
</p>
<pre>
# Imports not mentioned to maintain brevity

class PyFlakesResult(HiddenResult):

    def __init__(self, origin, deadScopes, pyflakes_messages):

        Result.__init__(self, origin, message='')

        self.module_scope = self.get_scopes(ModuleScope, deadScopes)[0]
        self.class_scopes = self.get_scopes(ClassScope, deadScopes)
        self.function_scopes = self.get_scopes(FunctionScope, deadScopes)
        self.generator_scopes = self.get_scopes(GeneratorScope, deadScopes)
        self.doctest_scopes = self.get_scopes(DoctestScope, deadScopes)
        self.pyflakes_messages = pyflakes_messages

    def get_scopes(self, scope_type, scopes):
        return list(filter(lambda scope: isinstance(scope, scope_type),
                           scopes))

    def get_nodes(self, scope, node_type):
        for _, node in scope.items():
            if isinstance(node, node_type):
                yield node


class PyFlakesASTBear(LocalBear):
    AUTHORS = {'The coala developers'}
    AUTHORS_EMAILS = {'coala-devel@googlegroups.com'}
    LICENSE = 'AGPL-3.0'

    def run(self, filename, file):
        """
        Generates pyflakes-enhance-AST for the input file and returns
        deadScopes as HiddenResult
        :return:
            One HiddenResult containing a dictionary with keys being
            type of scope and values being a list of scopes generated
            from the file.
        """
        tree = ast.parse(''.join(file))
        result = Checker(tree, filename=filename, withDoctest=True)

        yield PyFlakesResult(self, result.deadScopes, result.messages)
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
</style>
