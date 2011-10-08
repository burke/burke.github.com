---
layout: post
title: "Solving the Short vs. Long Command Prompt Tradeoff: Wünderprompt"
date: 2011-10-08 14:35
comments: true
categories: zsh, "workflow optimization"
---
For years, I was a believer in the short command prompt. Give me a
prompt character and maybe my current working directory, and I'm happy.
None of this extra nonsense that just makes the prompt slow and junks up
the display with stuff I have to ignore. If my commands wrap to the next
line, there's something wrong.

...Then I saw someone using a git-aware prompt, and my worldview
inverted. I added a few bits of git information to my prompt, and I was
hooked. I slowly started adding more and more context to my prompt. What
surprised me most was how, after a marker had been in my prompt for a
couple of weeks, I was suddenly constantly aware of its state without
even consciously looking at it. Just having the extra information
somewhere on screen added it to my mental model of the system.

...and so I started slowly collecting additional items in my prompt.
It got to the point where I was blown away by how intuitively I
understood exactly what state git was in at any given moment, but
two things started happening: my prompt got big; and my prompt got slow.

I could probably deal with a prompt that reaches halfway across the
screen, but when it takes 800ms to generate, it becomes a *huge*
workflow impediment. I had a predicament. Do I suck it up and deal with
a slow, long prompt, or remove some items? It turned out I could
actually get the best of both worlds, which is what led me down the path
to Wünderprompt. 

I spent some time looking at why the zsh git prompt libraries I had
written or borrowed were so slow. It turned out that most of the problem
was that I was shelling out to git over half a dozen time for *each*
propt generation. That's a lot. So I set out to fix it. 

I started writing a little utility that intuits as much as it can by 
inspecting the .git directory itself. Where my previous prompt would 
parse `git stash list` to determine whether there's a stash, it now
simply checks for the presence of `.git/refs/stash`.

As I was developing this new script, I remembered that once I was used
to the layout of a prompt, all the information was absorbed
subconsciously, so I laid it out as colourfully and efficiently (and
some might argue, cryptically) as possible. Without further ado:

<img src="/images/2011-10-prompt-1.png"/>

The `RPROMPT` is pretty uninteresting, so I'll skip over that. It's
mostly additional information I might care about, skimming back through
my history to find something.

The `PROMPT`, ie. the left side, is composed of six blocks, which I'll
go through one by one.

1. CWD: This is pretty simple. The last component of the path, colour
   coded by host. My laptop is cyan, one of my servers is red, another
   is yellow, and so on.

2. RVM/Ruby info: This is a bit dense. When I typed `turbo`, a red `X`
   was prefixed to this section. That indicates tuned GC settings,
   supported by REE and 1.9.3. These are implemented as environment
   variables, so I have them handled as a shell state. After the `X` is
   the name of my current gemset, colour-coded by the current ruby
   implementation. Purple is REE, white is 1.9.3, yellow is 1.9.2, and
   so on. If I'm using the default gemset, it shows an `@` instead.

3. Git commit harassment section: I added this after watching a
   screencast by Gary Bernhardt. It shows the time elapsed since the
   last commit in the current git project. 1-10 minutes is green, 10-30
   minutes is orange, and above that is red.

4. Git branch: Shows the current branch, replacing `master` with `*`. If
   the branch is clean, it's in green, and if it's dirty (ie. files have
   been changed since commit), it's purple.

5. Git status: This is essentially the M/D/A/? part of `git status`.
   If a symbol appears in the right column (ie. the working directory),
   it is shown in blue. If it appears in the left column (ie. the
   index), it is shown in black. If it appears in both, it is shown in
   red. You can see I modified a file, and a blue `M` appeared in the
   prompt. Later, in the blog directory, there is an added file (shown
   as `??` by `git status`, so it is represented by a red `?`. This
   section really takes some time to develop a feel for, but it's very
   handy to have the mental model without running `git status` all the
   time.

6. Prompt char and return status: Nothing too fancy here. `%` or `#` is
   shown, depending on whether I'm a user or root. It's green if the
   last command succeeded, and if it failed, it's red with the return
   value prefixed.

And that's my short long prompt. The best part? It generates in under
20ms. It's working wonderfully for me. The meat-and-potatoes bit is
available on [Github](https://github.com/burke/wunderprompt), and there are some
more random glue-y bits in my [zsh configuration](https://github.com/burke/dotfiles/blob/master/.config.d/zsh/prompts.zsh).
If you have any comments or suggestions, other than "refactor your
horrible C code", I'd love to hear them.
