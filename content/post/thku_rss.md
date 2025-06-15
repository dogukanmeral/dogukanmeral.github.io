---
title: THKÜ RSS
date: '2024-11-24'
---

Checking updates on announcement pages can get really annoying, especially if there are multiple of them and you have to check them periodically in order not to miss time-critical ones.

To get updates from the university I(how selfish) study at (THKÜ), I started writing THKÜ RSS generator. Basically it fetches HTML, parses it, creates XML feeds for each annoucement site (as I mentioned there are multiple announcement sites). 

how to get it?
=============

For now, cloning the [github repository](https://github.com/dogukanmeral/thku_rss) and building it using [hatchling](https://pypi.org/project/hatchling/) is the only option. Package will also be available in PyPI in near future (when I set up 2FA for PyPI in my non-lazy time).

how to use it?
=============

It's a terminal-based program so use your terminal emulator.

thku_rss -c
: Opens (c)onfiguration file in default text-editor. XML(feed) paths and log folder path must be changed for your own system. To add more feeds, create new dictionaries inside "feeds".

thku_rss -f
: (F)etches announcements from links in config.xml and creates XML feeds at specified paths.

Note: To schedule(automate) the program, use [cron](https://en.wikipedia.org/wiki/Cron) etc. 
To read content of feeds:
: I personally use [newsboat](https://newsboat.org/index.html) RSS reader, but you can use any RSS reader you want.

todo
====
* Setup a server to generate XML periodically(not that selfish)
* Set paths by default to home directory of user.
* Simplify configuration
* Create PyPI package

if you want to contribute
========================

Assuming you're a student/academician(!) in THKÜ(why would anybody else want to contribute) , you can help by:
* Suggesting IT and high-end academic people implementing this program into university's servers.
* Contacting me about your dislikes and suggestions at [dogukan.meral@yahoo.com](mailto:dogukan.meral@yahoo.com)
* pull requests (considering todos)
