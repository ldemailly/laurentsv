---
layout: post
title:  "My thoughts about Go (the language)"
#date:   2017-07-23 23:04:26 -0700
categories: blog
---

I have thoughts about [Go, the board game](https://en.wikipedia.org/wiki/Go_(game)) as well but for today, I'll focus on the computer language:

I learned a couple of programming languages over the years:

- Mainstream 'system' languages: C, C++, Java
- Mainstream scripting/others: Awk, Lua, Javascript, PHP, Perl, Python, Shell/Bash, Tcl
- Esoteric/other: Assembly (6502, 68k, x86 - I really prefer the Motorola to the Intel ways, but clearly Intel won), Basic, Cobol, Erlang, Forth, HTML/CSS, Lisp, Pascal, PL/1, Prolog (!), (La)Tex, XML

Does that makes me qualified to comment on a new (to me) language, well I hope so :-)

I started learning [go](https://golang.org/) in June 2017, 1 month is not enough to be proficient in anything
but I want to write down my first impressions before I forget and I get too used to the quirks:

- Tabs vs spaces
- Global variables
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
- issue with nested struct initialization
- good built in benchmark and easy profiler support
- package dependencies getting dragged in even if not used (eg grpc; probably through init()), leading to:
- big binaries

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
