+++
title = "My Favorite Vim Plugins"
date = "2011-06-03"
tags = ["vim", "programming"]
+++

It's been a long time since I wrote about Vim. In this post, I just wanted
to give an example of the plugins which make me love Vim as much as I do. On a
foreign machine, I certainly prefer Vim over any other command-line editor, but
since I'm so used to my settings, I am usually pretty slow at getting
(back) used to vanilla Vim. One of the greatest strengths of Vim is well known
to be its extensibility. You can really do a lot to make your life easier with
plugins. Below are a sample of the plugins that have come to quite strongly
define my experience with Vim.

## Plugins

### [BufExplorer](http://www.vim.org/scripts/script.php?script_id=42)

This is a pretty useful plugin but I really don't put it to good enough
use. Basically, the name describes a good deal of what the plugin does. It
allows you to very quickly switch between open buffers. (Remember to turn on
`hidden` so that when you close a file, you don't actually
kill the buffer, therefore allowing you to quickly open up the file with cursor
position, folds, etc. still intact through the course of a session). I know the
whole thing about using buffers over tabs and yeah yeah, okay, but I still use
tabs. I think in my brain it makes more sense to me and I'm going to stick
to it. However, whenever I do need to switch buffers, I can just do
`;:b` and let it auto-complete and switch to any buffer I want
quickly. Has saved me time pretty often.

### [Conque Shell](http://www.vim.org/scripts/script.php?script_id=2771)

Another one of my favorites but still pretty underused for me. This plugin fills
in a huge need: the ability to open a shell session inside Vim. This can be
invaluable for many things including checking on the progress on a task while
working on another task or allowing you to edit, compile, and run the
application in one window (although this can be accomplished with
`;:!` commands, this is more natural and allows you to see the code
at the same time. I usually work in Gnome-Terminal so I use terminal tabs for
this, but if you're in a PuTTY or SSH session, this can be useful. Very
straightforward to use and it works somewhat decently well, although not
perfectly.

### [NERDCommenter](http://www.vim.org/scripts/script.php?script_id=1218)

This is an extremely popular plugin from the NERD\* family and I use it daily.
This smart little plugin allows you to very quickly comment out a line of code
or create a new one. The kicker is that its the same key binding regardless of
what language you're working in, which improves muscle memory and saves a
ton of keystrokes (I'm looking at you, HTML). The built-in bindings seem
random and difficult but I'm used to typing `,cA` (with `<leader>` mapped to
`,`) that it's second nature to me. If you code a lot, I can't see why you
wouldn't use this plugin.

### [NERDTree](http://www.vim.org/scripts/script.php?script_id=1658)

This is possibly the most famous Vim plugin of all time. For good reason:
it's very powerful and complete and does a lot to boost the built-in file
explorer (`:Explore`). However, I do not put it to enough use because of my use
of wildmenus, which I consider faster and more intuitive coming from Bash.
However, there are times when the file I want to open is a few directories away
and I'll break open NERDTree in a split and find it manually. There are many,
many plugins that do this, but NERDTree is very polished and nice. I like it a
lot.

### [Obvious Mode](http://www.vim.org/scripts/script.php?script_id=2212)

I can't live without this thing. It's a very simple plugin that
changes the color of the bottom status bar to indicate when you're in
Insert Mode. I kept making the mistake of trying to edit the text of a file in
Normal Mode (tsk tsk, n00b mistake) so this was extremely useful. I set the
color to a bright orange so now I'm never confused. I guess it's a
crutch of sorts, but it improves my productivity so I like it. This is something
which I think can be built into Vim seeing how useful it is (maybe there is, not
sure). Note that you really don't need this plugin if you customize your
colorscheme file, but this is colorscheme-agnostic.

### [SnipMate](http://www.vim.org/scripts/script.php?script_id=2540)

So this is a very popular port of TextMate's code insertion feature and
it's very handy. I was recently writing some code and came across a TON of
situations where I wanted to print a status string like this:

```cpp
fprintf(stderr, "Some text message.");
```

That gets annoying to type after a while, but with this
plugin, I can just do `fpr<TAB><TAB>Some text message.` and I'm done. So
very useful. And don't get me started on for-loops. The very nice feature
about this plugin is that tabbing allows you to move between "fields" that hold
particular content. The annoying part of this is that it conflicts with the next
plugin, which makes me sad :(. Thus, another great plugin that I fail to use
enough.

### [SuperTab](http://www.vim.org/scripts/script.php?script_id=1643)

This is THE most used plugin out of anything I could ever do in Vim. Basically
since moving to this editor, I'd estimate that I actually type roughly
HALF of the characters in a source file. It's so, so much faster to just
code-complete partial strings that I never make the mistake of misspelling a
variable or function name and it saves a whole lot of time when you have long
function names. It's also pretty smart. Just a few days ago, I was writing
code that used the AES encryption and decryption functions provided in OpenSSL.
I just included the appropriate header file in my file and I was able to tab
through completions of function names in OpenSSL! Awesome and so useful. To be
clear, all of the functionality of this is available natively in Vim, except
that this plugin seamlessly switches between different kinds of completions with
ease and takes it out of your hair. The one disadvantage is that when you have a
very large (a few thousand lines) files with a ton of header files that include
a lot of standard library headers, kernel headers, etc., this can be pretty
slow. Apart from that, I love this thing.

### [Tagbar](http://www.vim.org/scripts/script.php?script_id=3465)

This final plugin is the newest one to my collection. I just got it a few weeks
ago and I use it a lot already. Basically, it provides a common feature in GUI
IDEs that lists all the function prototypes, global variables, etc. This is nice
because it lets you very quickly switch to a different function in the same
file. It also gives you the function prototypes at a glance so you can know
exactly how to call the function. It's also pretty smart - you
should see it at work in a header file with structs, extern variables, function
prototypes, macros, etc. Very cool. And it works in a ton of languages too which
makes it very useful.

## Conclusion

This is just a very small sample of the kind of power that Vim (and its
third-party developers) has to offer. There are a few other very popular ones
which I omitted here, like Pathogen. Surprising as it may be, I haven't
actually used that one. I guess I didn't realize I had these many plugins
that I actually _actively_ use. Perhaps it's time to look into that one
too. I'm always looking to improve my workflow so if I add many new ones,
I might another post about this in the future.
