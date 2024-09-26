---
layout: post
title:  "My thoughts on Go, part Ⅱ, 7 years later"
categories: blog
comments: true # they still work!
date: 2024-09-26
modified_date: 2024-09-26 10:40:00 -0700
---

7 years ago I had just started learning go and wrote down [my thoughts: part Ⅰ](/blog/2017/12/28/about-golang.html), it may be useful you quickly scan that previous post as I'm going to go in the same order more or less.

So let's go over what I learned since, what changed and newer thoughts from many years of professional as well as OSS golang experience:

- Tab vs spaces: gofumpt on save in vscode: life is good!

- Global variables: well they are to be avoided but the language does make it a bit too easy to write bad code some times.

- Visibility through casing of first letter: you get used to it, it's convenient/expressive, yet still annoying to change - but vscode does a good job at renaming symbols (but it does make for big diffs when you do)

- One binary per directory is still annoying and makes people use that horrible cmd/ convention (yes I don't like it! I'll make another post about overly complicated repo layout later but see references meanwhile)

- No const: that one is still annoying me... you can only have very primitive types as const, not even arrays or struct. And you can't declare that your function isn't going to mutate the arguments either. What's so hard about putting more stuff in text segment... or checking what the function does... Yes I saw the FAQ... While the go team does amazing job most of the time, they like everyone (me too, clearly!) have blind spots. Anyway, one saving grace is that passing mid size structs by value is actually not expensive and the compiler can optimize. And passing by value is the best way to guarantee const (well... except slices etc...)

- `&local` you get to embrace it... and then never use `new`.

- 7 years later I still often put the type first (`int foo`), get my IDE all red, and then fix it...

- It is super super annoying that if you embed a struct you can't set fields without super verbose reference to the inner types... but there is a proposal to fix that, hopefully it happens before I die of old age.

- I still don't like my go.mod showing testing dependencies of other packages...

- go mod tidy is great... except since go 1.21 the go team (see above) finally fixing their bad release numbering (ie go1.20 for first release followed by go1.20.1 first patch... hello semver... now it's good, go1.23.0 was the first one) screwed up the `go` line of go mod which should have remained (and hopefully maybe they'll revert) a "language version" and thus without an ugly patch level in there.

- Use CI to run the latest patch but don't force everyone using your library or code to upgrade (talking about you go.mod lines like `go 1.23.0` prematurely)

- Binaries are still big, there is tinygo but it's not really "working" (made a bunch of PRs to sort of finally got it working and yet not quite, gave up for now)

- Naming conventions still make for ugly stuff (but then... you don't have to agree with them or can make linter exceptions). In particular I like MY_CONSTANTS to stand out but I'm getting used to MyConstants to... not.

- `append()` embrace it, it's in bed with the GC/allocator to make things efficient.

- jar/gar: happened since I wrote the original and is great, it's `go:embed`, along static linking it allows you to send full fledged applications in a single binary, including web assets etc... (I use that for fortio, it was a god send compared to before)

- forking/hosting/lack of working with go install replace: it's still a big pain to wait for upstream fix, forces you pretty much to change all the imports to the fork...

- debug.BuildInfo is great... except `go build` doesn't fill it up, and `go install` doesn't take `-o`, it makes for annoying gymnastic when building. but `goreleaser` is great.

- GC/"managed memory" and yet you can accidentally allocate 10x the 'soft' GOMEMLIMIT and not even get a panic (you will get OOM killed though)... annoying... had to jump through hops and measure/protect my own allocs in [grol code](https://github.com/grol-io/grol/pull/159)

- You get used to IDEs to get to know the type of what mysterious function Foo() might return, but that doesn't work when someone paste code in slack or discord... so I would still have preferred the non `auto` style variables: optimize for reader not writer... but... I do write more than I read these days so... maybe not.


What I also learned since:

- Use linters, and have a good configuration (hard!) for them: it's a lot easier for a coworker to get told by machine to make a change than to argue about it with another person (same thing as formatting, about consistency, and readability it gives you). My favorite so far


- Annoyance: even with grouped dependabot, dependencies update is quite a chore. But govulncheck, dependabot, linters, bug fixes, etc... help keeping our ecosystem healthy and it has to get done.

- golangci-lint: (mostly) great, but quite an horrible name... I can't for the life of me remember if it's golanglint-ci (like maybe it should) or some other variations... thankfully gol<tab> works when I need.

### References

- Gofumpt the better go fmt: [github.com/mvdan/gofumpt](https://github.com/mvdan/gofumpt)
- Shared workflows [github.com/fortio/workflows](https://github.com/fortio/workflows)
- [golangci-lint](https://golangci-lint.run/) config [github.com/fortio/workflows/blob/main/golangci.yml](https://github.com/fortio/workflows/blob/main/golangci.yml)
- [goreleaser](https://goreleaser.com/)
- Keep your repo simple, don't over do packages, directories, etc: [go-standard/project-layout](https://github.com/go-standard/project-layout#project-layout) (though I don't agree with agile but the conclusions there are still better than most overly complicated recommendations you can sadly find all over)
- [github.com/fortio/version](https://github.com/fortio/version) and the rest of the fortio ecosystems I hope you can find useful (cli and server shells, dynamic flags, sets, safecast, terminal stuff, etc etc..)
- [grol.io](https://grol.io/) open source scripting language go syntax like, Tcl spirit like, I've been having fun writing.

### But...

What do you guys think ?

Ping me on gopher slack (Laurent Demailly) or discord (_dl) if you disagree, have comments etc... or open an [issue](https://github.com/ldemailly/laurentsv/issues)
or comment directly below if you're on facebook.
