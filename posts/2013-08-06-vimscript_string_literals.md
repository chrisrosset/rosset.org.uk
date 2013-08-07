----
title: Vimscript's String Literals
tags: vimscript vim
----

## Introduction

Vimscript appears to be have two basic types of string literals.

1. Double quoted literals.

These are very similar to string literals found in other languages. They are documented in `:help expr-string`.

2. Single quoted literals.

These allow using almost any character without using escaping. The only character that requires escaping is the single quote (') character itself. And the escape character is... another single quote. For example:

    'this is a single quoted literal with a '' character inside'

These are documented in `:help literal-string`.


## The problem

Now, these are fairly sensible (even if using the single quote character for escaping is questionable). There is, however, one more type of strings in Vimscript: string option arguments.

String options (such as *encoding* or *colorscheme*) take a string argument. In order to minimise the amount of typing required, using any quotes in this case is unnecessary. Actually, not only is it unnecessary -- it's very much wrong.

For example, it might seem that these two commands should result in the same:

    set formatoptions=qrn1
    set formatoptions="qrn1"

This is not the case though. The first works as expected. 

    :set formatoptions=qrn1
    :echo &formatoptions
    >qrn1

But what's the result of the latter?

    :set formatoptions="qrn1"
    :echo &formatoptions
    >

Nothing? Yes, nothing. This happens for two separate reasons. One is that Vimscript requires using this special no-quotes format for setting options. You can see this clearly, if you try using the single quoted literals.

    :set formatoptions='qrn1'
    >E539: Illegal character <'>: formatoptions='qrn1'

Why does it not complain about the double quoted string? Surely that must be illegal as well.

Well, it just so happens that Vimscript end-of-line comments happen to start with a double quote sign. Thus, the entire "string" is just ignored as a comment and we end up with an empty string being passed as the option argument.
