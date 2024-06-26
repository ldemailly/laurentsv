---
layout: post
title:  "Go docker images: small and simple"
categories: blog
comments: true # they still work!
---

Ok folks, I'm writing this a bit in anger from seeing people struggle with making very small and very safe docker images, and also overly complicated go project structures.... Stop the madness!

Background: go is an ideal language and platform for making multi OS multi cpu architecture tools and servers - and when you deploy or share your servers through a Docker image (to be used in, say, kubernetes with `containerd`), the smaller the image the better, additionally the fewer files also the better, in particular not having any shell, etc... makes images that much more secure

So while using `FROM scratch` is not new at all (blogs from 8 years ago mentioned it, that's when I started using it), it has become harder to find an up to date yet terse example.

So here we go, minimal stuff:

```Dockerfile
# This as of this writing will pick up 1.22.4 - consider adding sha for security
FROM golang:1.22 as build
WORKDIR /build/src
# Splitting this makes it a cached layer of your dependencies - it's optional
COPY go.mod go.sum .
RUN go mod download
# Copy the (rest of the) source tree
COPY . .
# Real stuff - statically linked, stripped, pure go build
# Assumes what you build is in the top level, like it should,
# don't go and add cmd/foo/main.go until you really have more
# than 1 binary and even then there is probably a main one
# having cmd/foo also makes `go install github.com/yourname/yourepo@latest`
# not work (or be longer than necessary) when you don't put your main at the top,
# but if not and if you must replace . (current package) by ./cmd/foo/ next line:
RUN CGO_ENABLED=0 go build -ldflags="-a -s -w" -o app .
# This is the important bit
FROM scratch
COPY --from=build /build/src/app /usr/bin/app
ENTRYPOINT ["/usr/bin/app"]
```
Save as Dockerfile in your top level repo directory, where go.mod etc should be and

```shell
docker build -t local_build .
```
Rejoice at the small size:
```
$ docker images local_build
REPOSITORY    TAG       IMAGE ID       CREATED         SIZE
local_build   latest    96f0a048d99c   2 minutes ago   13.1MB
```
Run it/make sure it works:
```
docker run -ti local_build
```

Note that if you make https out calls for instance, you'll want to put:
([example](https://github.com/fortio/cli/blob/v1.6.0/ca_bundle.go#L14))
```
import _ "golang.org/x/crypto/x509roots/fallback" // CA bundle for FROM Scratch
```
in your main - or use [fortio.org/cli](https://pkg.go.dev/fortio.org/cli) for tools or `scli` for servers, which does it (and more!) for you. (allowed me recently this [simplification](https://github.com/fortio/multicurl/pull/146/files#diff-dd2c0eb6ea5cfc6c4bd4eac30934e2d5746747af48fef6da689e85b752f39557))

And possibly copy a /etc/mime.types from the base - see the excellent [Xe's adventure](https://xeiaso.net/blog/2024/fixing-rss-mailcap/) about that file.


If you want proper version embeded and easy multiarch, consider the excellent go releaser, you can see how that is used at [github.com/fortio/multicurl](https://github.com/fortio/multicurl#multicurl) and [github.com/fortio/workflows/blob/main/.github/workflows/releaser.yml](https://github.com/fortio/workflows/blob/main/.github/workflows/releaser.yml)

Ping me on gopher slack (Laurent Demailly) or discord (_dl) if you disagree, have comments etc... or open an [issue](https://github.com/ldemailly/laurentsv/issues)
or comment directly below if you're on facebook.
