---
layout: post
title:  "No nonsense guide to Go projects layout"
categories: blog
comments: true # they still work!
date: 2024-10-19
modified_date: 2024-10-19 14:19:47 -0700
---

It's a recurring question on gopher slack and discord: "How should I setup my go project repository?".
And unfortunately there is a *lot* of both outdated and overly complicated blogs and sample or old projects out there.

One main philosophy for go, is keeping things simple. More code writing, less structuring ahead of time / before it's actually needed.

### Official

The official guide on this topic is [go.dev/doc/modules/layout](https://go.dev/doc/modules/layout) yet people still tend to over complicated, it says

> **Larger** packages or commands **may** benefit from splitting off some functionality into supporting packages. **Initially,** it’s recommended placing such packages into a directory named internal; [...] Since other projects cannot import code from our internal directory, we’re free to refactor its API and generally move things around without breaking external users.

Emphasis on _larger_ and _may_ is mine. And seems to be missed by many. 99% of people do not need `internal/` when, be real, most of what gets written will never be reused much and even if it is, don't worry about over publishing. And the initially is actually in my opinion wrong... initially... make something useful and don't worry that it'll get such great adoption that you're stuck into maintaining your early layout forever, you're not... (see semver point below too).

Likewise, you don't need to use `cmd/` when you only have 1 binary (or even a few and no library)

In general because a feature exists, doesn't mean to have to use it (e.g `internal/`). Or because some people have a convention doesn't mean you should follow (like your mom probably used to say "if they jump off a bridge... will you?", remember? ).

By the way, stick to 0.x semver (another pet peeve of mine is the v2/v3/... in go) for as long as you can and document what you change and why, rather than under publish and force people to fork to access what they really need and you didn't forsee.

### The bad

So one infamous example of the nonsense is the fake "golang-standards" - see [github.com/golang-standards/project-layout/issues/117](https://github.com/golang-standards/project-layout/issues/117) and other closed issues for why it's _not_ a standard. Which led folks to come up with a counter point at [github.com/go-standard/project-layout](https://github.com/go-standard/project-layout) (which I contributed a few additions/corrections) but also got slack for having the word "standard" in it... so here you go, below is _my_ **opinion** (based on decades of professional experience in many languages, including 7 years of go at top companies and OSS projects) - Some of the text comes from there:

### My Take

#### Where should main go?

Main package in the root is great, it makes for the shortest and cleanest install/run:
```sh
go install github.com/you/project@latest
```
Which is typically the priority if you do have a cli or server to ship. Pure libraries obviously have no main at the root.

If you do have both, you could move the main out of the way in a child directory (people like `cmd/execname/` but the `cmd/` part is actually not needed) or move the library into a directory that will make importing and using it readable. Consider also you may have more than 1 library package to publish in which case `lib1/` and `lib2/` makes total sense and keep the top level for the main binary.

For many small to medium size projects these are the only packages you need.

#### internal/

You don't need that, unless you're shipping code to _a lot_ of third party users and have a lot of code that can't just be lower case not exported, yet used in many exported packages, you don't need to hide your code... it's not so precious or such a huge commitment.

#### pkg/

Any package you put into a pkg directory can be moved the root.
`pkg` is a very outdated convention from before `internal/` (and you saw what I think about internal/).

Some people think that having packages at the top, "clutters", the directory... first what does get cluttered if you add unnecessary directory layers is every single import in dozen of files and possibly hundreds of dependencies... think about them... Second, feel free to move the various docs etc to some subdirectory. Lastly what is 'clutter' navigation probably comes through your "godoc".

Btw don't get me started with repos that have dozens of files and that to see what is going on not using an idea you have to go main.go -> some other file -> some init file -> some start function -> some other files... while all this could be in 1 or 2 places, easier to follow and find.

#### util/

Those functions, types, and methods which you think belong in a separate `util/` (or `common/`, or `shared/`, or `lib/`) package simply don't.

But Laurent, I'm more comfortable with...

Let me guess, you have more experience writing Java, Python, Ruby, JavaScript, TypeScript, or C# than you do writing C, C++, or Perl. Please consider that package structuring is more an art than hard rules.

#### Too many packages

You can have more than 1 file in a package/directory in go. You don't need to make a new package each time to separate code/types/... And the more packages the more chances to create a loop or having to later move code around. This being said, moving code around is good and an easy way to grow your code base - later, when needed.

#### Examples

There are lots of example of overly complicated repos. I'd give my own repos but they also evolved organically and aren't actually that good/or simple, but here we go anyway:

Packages in [github.com/fortio/](http://github.com/fortio/)

Server example:
- [github.com/fortio/proxy](https://github.com/fortio/proxy)

Cli example:
- [github.com/fortio/multicurl](https://github.com/fortio/multicurl)

Server + libraries + cli:
- [github.com/fortio/fortio](https://github.com/fortio/fortio)

Library + clis:
- That one is somewhat odly setup as it has 2 libraries, one at the root where it started and a newer one, so not exactly best [github.com/fortio/terminal](https://github.com/fortio/terminal)


### But...

Ping me on gopher slack (Laurent Demailly) or discord (_dl) if you disagree, have comments etc... or open an [issue](https://github.com/ldemailly/laurentsv/issues)
or comment directly below if you're on facebook.

See also / you might like [my other thoughts on go](/blog/2024/09/26/golang-7-years-later.html)
