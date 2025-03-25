# Laurent Silicon Valley

Laurent sharing his experience as a Silicon Valley veteran

Content on this site is Copyright 2017-2025, All Rights Reserved, by Laurent Demailly.
Do not redistribute - links to content are welcome.

This repo is the source for https://LaurentSV.com/ - everything is in [docs/](docs/)

Started as jekyll minima v2 and converting to v3

Local testing:

```sh
cd docs
bundle exec jekyll serve
```

Notes about conversion: (might help others)

In Gemfile:
```Gemfile
gem "minima", github: "jekyll/minima"
```

and then
```sh
bundle install
```

Issues:

- icon-github.html and twitter/x.com icons:
```html
            <a href="https://github.com/{{ site.github_username }}">
              <svg class="icon"><use xlink:href="{{ '/assets/minima-social-icons.svg#github' | relative_url }}"></use></svg>
            </a>
```
See v3/ PR

There is probably more but this seems to be working/rendering about the same as before, with auto dark mode.
