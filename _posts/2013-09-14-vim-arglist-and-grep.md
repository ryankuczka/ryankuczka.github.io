---
layout: post
tags: vim grep
title: Vim Arglist and Grep
---
So today, I needed to perform a basic find and replace across many
(hundreds) of files. In Vim, it is easy to replace all instances of
`foo` with `bar`. In fact, it's as simple as

```
:%s/foo/bar/g
```

However, this is only within one file, so to perform this action on
multiple files, you need to use the arglist.

<!--more-->

If you start vim from the command line with files as an argument, those
files are automatically added to the arglist. You can also add to the
arglist using the command `:argadd` from inside vim, but the problem
with this method is that adding to the arglist manually can be annoying
when you have a lot of files that also cannot be added using standard
pattern matching and wildcards (e.g., `path/to/files/*.html`).

The beauty of the arglist is that once you have all the files you want
in it, you can then do

```
:argdo %s/foo/bar/g | update!
```

The **argdo** will perform whichever command that follows on each file
in the arglist. The argdo command however does not write and save the file.
Therefore we follow up with the **update** command (followed by a `!` to
force it to save, just in case you are prevented for some reason).

I knew that I could **grep** for the term I wanted to replace and end up
with a list of the files containing this term by passing the `-l` flag.
So I started looking into how I could get that list into the arglist in vim.

You can't just simply pipe the output of the **grep** into the **vim**
command, but luckily Unix also has the **xargs** command which can turn the
**grep** output into arguments. This means that you can do

```
grep -l foo | xargs vim
```

and then vim will open up with all those files in the arglist, then you
can run the command above and replace all instances of `foo` with `bar`
in your entire codebase.
