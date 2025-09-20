+++
title = "Hello Hugo"
author = ["Brandon C. Irizarry"]
date = 2025-09-18
lastmod = 2025-09-20T13:16:05-04:00
tags = ["hugo", "org"]
categories = ["cool"]
draft = false
summary = "Hugo, Ox Hugo and GitHub Pages → this website."
+++

## Introduction {#introduction}

After floundering about unsuccessfully with Hugo in the past, this
time things finally clicked. One thing that helped me greatly here
was becoming acquainted with Go; and so, for example, things that
looked strange before, like Hugo templates, are now a familiar
idiom.

It's remarkable the degree to which software engineering skills are
built upon a foundation of familiarity with lore.[^fn:1] After
becoming familiar with backend web development, the idea of
"starting a local server" (for example, to preview one's website)
was no longer as weird as it was before.

I was overjoyed to find out that Hugo supports Emacs Org Mode
natively! However, I quickly found out[^fn:2] that the recommended
approach is to use Ox Hugo, something else with which I had toyed
with but ultimately failed to understand - and, as in the case of
Hugo itself, finally grokked.


## Some Backstory {#some-backstory}

I needed a website - the one you're reading now - because, at the
time of this writing, I'm looking for a full-time software
developer position.  Now, without falling into some sort of
pernicious Dunning-Kruger tarpit, I can assert confidently that, in
my university years as a CS major in the early 2000's, I was _bad_
at programming; I was arguably one of the worst in my
class. Functional programming? Forget it: recursion was completely
over my head. Low-level? Had no idea how to write C. You get the
idea.

However, over the years, something clicked where I felt I really
needed to get back into the loop of coding; after all, I had this
degree, but had never used it. Such a discrepancy began to weigh
down on me, and, once I started having a bit of free time here and
there), I began the effort to reform myself according to what I had
professed previously as my declared profession.

Interestingly, one thing that my university years _did_ give me was
a taste for Emacs. This occured while taking an Operating Systems
course, where the lab computers were outfitted with Emacs, which we
would use to edit our source files for our projects. Now looking
back at that time, I suppose it was enlightened of them to "make
us" use Emacs. To this day, when editing code, I make use of a
shortcut for copying a line a fellow lab user had shown us: `C-k`
followed by a quick `C-y` to first cut away the line and then
quickly paste it back where it was, so that it would remain in
Emacs's kill ring for pasting in somewhere else.

And so, in recent years I've been brushing up on old skills, and
building new ones. I've picked up acquaintance with quite a few
languages out there, some feverishly popular, and some less
popular.

One such language was Lua. I have a soft spot for Lua. Lua 5.3 was
the first programming language I learned _well enough_ to write
serious, lengthy programs in[^fn:3]. The book _Programming in Lua_[^fn:4] is
fantastically well-written, arguably on par with K&amp;R in this
respect. The chapter on Lua patterns primed my brain to finally
truly understand regular expressions later on.

Programming can be hard at first, but practice and determination
really do make perfect.


## Enter Hugo {#enter-hugo}

As I stated earlier, I was now able to hit the ground running with
Hugo, while writing my blog posts in a format I enjoy:
Org. Markdown, while it gets the job done, feels in comparison a
bit unwieldy and lacking[^fn:5]. Nevertheless, it would seem that Markdown
is itself a kind of accepted _lingua franca_ for developers
nowadays, and so I resort to it when the occasion calls.

There's an excellent blog post[^fn:6] that details getting set up
with Hugo. The author frames his explanation in the context of
Windows and PowerShell, but I nevertheless found the essential
instructions quite clear. Let's of course not forget the
documentation at [gohugo.io](https://gohugo.io).

For people interested in Emacs and Ox Hugo, there's also
[ox-hugo.scripter.co](https://ox-hugo.scripter.co/).

I'll now detail a quick summary of the steps I took.


### Install Hugo {#install-hugo}

Specifically, the Extended Edition (though I went full maximalist
and installed the Deployable Edition.)


### Create a New Hugo Site {#create-a-new-hugo-site}

The site you create in this step will be your actual blog. Hugo,
at heart, is a Golang command-line app, and so drives like one:

`hugo new site $SITENAME`


### Initialize a Git Repo at the Root of Your Site {#initialize-a-git-repo-at-the-root-of-your-site}

At this step, you'll also add a nice `.gitignore` file:

```text
.DS_Store
.hugo_build.lock
/public/
/resources/_gen/
```


###  {#d41d8c}

[^fn:1]: Arguably, this is why a lot of people famously find coding
    difficult at first: there's a lot of _implied_ know-how one really
    needs to come to the table with in order to be successful at the
    endeavor. Proficiency in matters of software can often be a sub-linear
    bootstrapping process.
[^fn:2]: <https://weblog.masukomi.org/2024/07/19/using-org-mode-with-hugo/>
[^fn:3]: I had actually used it to complete the _Nand to Tetris_ course
    on Coursera. Perhaps not as popular a choice as, say, Java or Python,
    but hey - I was more interested in Lua at the time.
[^fn:4]: Lua 5.0 edition available here: <https://www.lua.org/pil/contents.html>
[^fn:5]: However, Markdown Mode for Emacs is nevertheless excellent, and
    can make editing Markdown almost feel like you're editing Org!
[^fn:6]: <https://mikefrobbins.com/2023/10/26/building-and-deploying-a-blog-with-hugo-and-github-pages/>
