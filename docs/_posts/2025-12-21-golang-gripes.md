---
layout: post
title:  "Summary of my issues with Go (golang)"
categories: blog
comments: true # they still work!
date: 2025-12-21
modified_date: 2025-12-21 10:33:30 -0800
---


Go is so far my favorite programming language. But that doesn't mean I love everything about it. What my gripes are comes up from time to time and I tend to forget some or it is scattered among my 2 other posts about the language ([my thoughts: part Ⅰ](/blog/2017/12/28/about-golang.html) and in particular the newer [part Ⅱ](/blog/2024/09/26/golang-7-years-later.html), I repeat a number of the issues listed there here) so I decided to gather a list in one place that I can share:

- Having added a useless digit to the go line on go.mod. go line should have remained the language version not a specific patch level that everybody now needs to set to .0 anyway.
- golang.org/x forcing cascading updates by bumping said go line even without any other change actually using the new features. Libraries shouldn't force upgrades without benefits (which isn't to say you shouldn't use a stable/supported version to build final binaries)
- completely useless `toolchain` in go.mod and `go mod tidy` adding it / making unnecessary changes.
- Enums (or at least whitelisted go:generate for stringer and other trustworthy tool so one doesn't need to commit to source control generated files)
- Unions
- goroutineid not exposed (and bad justification for that in the go faq, that reads like bad faith). It's very useful for logging.
- generics are half baked. having type switches is super ugly
- improvements that are a bit silly like `for range 42` (but not `for range start:end:step`) or iterators instead of the above
- size of binaries
- go playground and go doc not doing basic syntax highlighting because some early go team member don't like colors.
- go doc preview improved and yet still not working
- Need `const` structs, maps, arrays, slices as well as function signatures
- Small stuff like not having `d` days in durations.
- Bigger stuff like the date/time format.... 2006/... (and the us centric order too)... what is wrong with people to come up with such an idiotic non standard way
- Visibility by upper/lower case of first letter is still weird 9 years later
- `go install -o`
- Embedded struct init
- Testing dependencies showing up in go.mod
- A pain to to use a fork while waiting for a bug fix to get upstream
- GOMEMLIMIT being so soft you can ask for 10x
- `:=` <-> `=` dance when moving code

And again, despite these, go is the most productive and enjoyable language for me.

And also things other people do gripe about and I think are fine:

- `nil`
- error handling
