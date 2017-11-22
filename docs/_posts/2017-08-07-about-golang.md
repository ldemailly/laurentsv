---
layout: post
title:  "My thoughts about Go (the language, aka golang)"
#date:   2017-07-23 23:04:26 -0700
categories: blog
---

I have thoughts about [Go, the board game](https://en.wikipedia.org/wiki/Go_(game)) as well but for today, I'll focus on go the computer language (aka golang):

I learned a couple of programming languages over the years:

- Mainstream 'system' languages: C, C++, Java
- Mainstream scripting/others: Awk, Lua, Javascript, PHP, Perl, Python, Shell/Bash, Tcl
- Esoteric/other: Assembly (6502, 68k, x86 - I really prefer the Motorola to the Intel ways, but clearly Intel won), Basic, Cobol, Erlang, Forth, HTML/CSS, Lisp, Pascal, PL/1, Prolog (!), (La)Tex, XML

Does that makes me qualified to comment on a new (to me) language, well I hope so :-)

I started learning [go](https://golang.org/) in June 2017, 1 month is not enough to be proficient in anything
but I want to write down my first impressions before I forget and I get too used to the quirks:

- Tabs vs spaces: it's annoying at first, but actually great as long as there are no mix of spaces and tabs: 100% spaces is my preference because what you see is what you get, but I learned to like 100% tabs so I can set my tab to 2 spaces and everything looks good (except on github or if I was to print, but who prints nowadays)
- Global variables
- Visibility through first letter... makes for a lot of unecessary changes when you change your mind on the scope of a function - thank god for global search and replace, but quite sad when looking at a PR/diff/CL with all the noise
- Annoying directory/file conventions/limitations
- No const !!
- Unnatural legal &localObject idiomatic way to construct instances
- Cool automatic interface implementation
- Pretty good concurrency/performance
- Not great performance
- Good way to write tests
- Annoying missing super basic like CheckEquals
- var syntax (and type after var) tripping me up still
- cute  var ( ... multi line list.. ) syntax (ditto for import)
- issue with nested struct initialization: can't initialize flat
- good built in benchmark and easy profiler support
- package dependencies getting dragged in even if not used (eg grpc; probably through init()), leading to:
- big binaries
- some silly conventions like having to rename DynamicHttpServer to DynamicHTTPServer (what if it was an https ...) and other un-_readability_ features
- -x missing in slice access (from the end)
- append/push_back (mutate array); ... wtf
- TODO: look at GC (why is there one? why not just like c++ sharde_ptr)
- new vs make vs &local (!)
- can't fork/demonize
- optimize for code reader/reviewer's time, not the writer's : Why not an IDE to fill in types instead of := and C++'s `auto`
- TODO: look at rust
- where is my thread local storage
- todo: embedded go
- 'jar' (gar?)
- hosting or forking a go library shouldn't require changing the source code (the package import)
- Dependencies
- linter versioning and cost to lint in CI


Testing source code feature:
{% highlight go %}
// EscapeBytes returns printable string. Same as %q format without the
// surrounding/extra "".
func EscapeBytes(buf []byte) string {
	e := fmt.Sprintf("%q", buf)
	return e[1 : len(e)-1]
}

// DebugSummary returns a string with the size and escaped first max/2 and
// last max/2 bytes of a buffer (or the whole escaped buffer if small enough).
func DebugSummary(buf []byte, max int) string {
	l := len(buf)
	if l <= max+3 { //no point in shortening to add ... if we could return those 3
		return EscapeBytes(buf)
	}
	max /= 2
	return fmt.Sprintf("%d: %s...%s", l, EscapeBytes(buf[:max]), EscapeBytes(buf[l-max:]))
}
{% endhighlight %}
