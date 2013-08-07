----
title: Vimscript Option Names
tags: vimscript vim
----


## Introduction

One of the main design issues with Vimscript is that is serves two, very distinct purposes. On one hand, Vimscript is the language used interactively by the user in an editing session. On the other, it's used for implementing complex functionality in the form of plugins.

This dichotomy has had profound effects on the language. These can be clearly seen when the design goals for the different purposes are considered.


## The problem

Short-hand option names are a great example of this issue.

Depending on what the user is working on, they might choose to modify the Vim environment by the use of options. One of the ways to do that is to execute a set command. For example:

    set number
    set tabstop=4
    set textwidth=80
    set lazyredraw

However, in an attempt to minimise the tediousness of typing out these long names, Vim is happy to accept abbreviations. Thus, the following would have the same effect as the example above.

    set nu
    set ts=4
    set tw=80
    set lz

While these are indeed somewhat handy during regular editing, it not uncommon to find these in scripts (even though such use is discouraged). These are rather unreadable as they are somewhat irregular and, most of the time, require consulting Vim help files.

This, perhaps, could be allevated if the abbreviations weren't part of the language. These could exist inside of some autocompleter (available when running the editor itself) so that they're still available in reasonable scenarios.

This, however, is not the case and is one more issue that makes Vimscript less readable and more prone to errors.
