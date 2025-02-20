---
title: simply lovely todo.txt
date: '2024-11-25'
---

[todo.txt](http://todotxt.org/) is a super-simple way for personal todos. Does only one job and does it well. It being only a bash(.sh) file makes installing it easy and hackable (not in a malicious way).

There are lots of "community apps" based on todo.txt both graphical and terminal-based. And there is also an Android app. But in this post I'm going to explore raw-dog todo.txt CLI.

let's install
============

1. Download latest package(.zip or .tar.gz) from [releases page](https://github.com/todotxt/todo.txt-cli/releases).
2. Extract those files.
3. Create an alias for .sh file in .bashrc by adding lines:

`alias todo="<path_to_todo.sh>"` (for that change to take effect, restart your terminal-emulator or run `source ~/.bashrc`)

todoing
======

I will keep it as simple as possible. For [all capabilities](https://github.com/todotxt/todo.txt-cli/wiki/User-Documentation) of todo.txt. Or just simply run: `todo help`

Adding tasks:
<img src="/images/todo.txt/add_task.png">

Adding tasks with +project and @context tags: 
<img src="/images/todo.txt/add_task_pri_proj.png">

Listing all tasks:
<img src="/images/todo.txt/list_all_tasks.png">

Listing using regular expressions:
<img src="/images/todo.txt/list_regex.png">

Listing by +project and @context:
<img src="/images/todo.txt/list_pri_proj.png">

Marking task as done:
<img src="/images/todo.txt/done.png">

Replacing task text:
<img src="/images/todo.txt/replace.png">

for todo maxers
==============

Developing bash scripts on top of todo.sh or auto-self-mail sender thingy for .txt file are on my todo list.
