---
layout: post
title:  "My thoughts about Go (the language, aka golang) - part 1"
#date:   2017-07-23 23:04:26 -0700
categories: blog
---

I have thoughts about [Go, the board game](https://en.wikipedia.org/wiki/Go_(game)) as well but for today, I'll focus on go the computer language (aka [golang](https://golang.org/)):

Some background [about]({{home}}/about/) me: I learned a couple of programming languages over the years:

- Mainstream 'system' languages: C, C++, Java
- Mainstream scripting/others: Awk, Lua, Javascript, PHP, Perl, Python, Shell/Bash, Tcl
- Esoteric/other: Assembly (6502, 68k, x86 - I really prefer the Motorola to the Intel ways, but clearly Intel won), Basic, Cobol, Erlang, Forth, HTML/CSS, Lisp, Pascal, PL/1, Prolog (!), (La)Tex, XML

Does that makes me qualified to comment on a (fairly) new (to me) language, well I hope so :-)

I started learning [go](https://golang.org/) in June 2017, a couple of months is not enough to be proficient in anything
but I want to write down my first impressions before I forget and I get too used to the quirks (I started compiling this about 1 month into my learning):

Here is a chronological list/menu - let me know which you'd like me to expand upon:

- Tabs vs spaces: it's annoying at first, but actually great as long as there are no mix of spaces and tabs: 100% spaces is my preference because what you see is what you get, but I learned to like 100% tabs so I can set my tab to 2 spaces and everything looks good (except on github or if I was to print, but who prints nowadays - save the trees!)
- Global variables
- Visibility through first letter... makes for a lot of unecessary changes when you change your mind on the scope of a function - thank god for global search and replace, but quite sad when looking at a PR/diff/CL with all the noise
- Annoying directory/file conventions/limitations (can only have 1 binary/main package per directory - wtf)
- No const !!
- Unnatural legal &localObject idiomatic way to construct instances
- Cool automatic interface implementation
- Pretty good concurrency/performance
- Not great performance
- Good way to write tests
- Yet annoying missing super basic like CheckEquals
- var syntax (and type after var) tripping me up still
- cute var ( ... multi line list.. ) syntax (ditto for import)
- issue with nested struct initialization: can't initialize flat
- good built in benchmark and easy profiler support
- package dependencies getting dragged in even if not used (eg grpc; probably through init()), leading to:
- big binaries
- some silly conventions like having to rename DynamicHttpServer to DynamicHTTPServer (what if it was an https ...) and other un-_readability_ features
- -x missing in slice access (from the end)
- append/push_back (mutate array); ... wtf
- TODO: look at GC (why is there one? why not just like c++ shared_ptr)
- new vs make vs &local (!)
- can't fork/demonize
- optimize for code reader/reviewer's time, not the writer's : Why not an IDE to fill in types instead of := and C++'s `auto`
- TODO: look at rust
- where is my thread local storage !?
- TODO: embedded go
- 'jar' (gar?)
- hosting or forking or moving a go library shouldn't require changing the source code (the package import)
- Dependencies !
- linter versioning and cost to lint in CI

What do you guys think ?
