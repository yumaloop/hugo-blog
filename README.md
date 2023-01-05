# hugo-blog

## Depoly

```
$ hugo
hugo: downloading modules …
hugo: collected modules in 4605 ms
Start building sites …

                   | EN
-------------------+------
  Pages            | 116
  Paginator pages  |   4
  Non-page files   |  12
  Static files     |   7
  Processed images |  12
  Aliases          |  39
  Sitemaps         |   1
  Cleaned          |   0

Total in 5543 ms
```

```
$ hugo server
Start building sites …

                   | EN
-------------------+------
  Pages            | 116
  Paginator pages  |   4
  Non-page files   |  12
  Static files     |   7
  Processed images |  10
  Aliases          |  39
  Sitemaps         |   1
  Cleaned          |   0

Built in 561 ms
Watching for changes in /Users/xxxx/xxxx/yumaloop.github.io/{assets,content,data,static}
Watching for config changes in /Users/xxxx/xxxx/yumaloop.github.io/config.toml, /Users/xxxx/xxxx/yumaloop.github.io/config/_default, /Users/xxxx/xxxx/yumaloop.github.io/go.mod
Environment: "development"
Serving pages from memory
Running in Fast Render Mode. For full rebuilds on change: hugo server --disableFastRender
Web Server is available at http://localhost:1313/ (bind address 127.0.0.1)
Press Ctrl+C to stop
```

## Debug log

### Homebrew (on MacOS)

```
$ brew update
$ brew upgrade
$ brew cleanup # <-- too heavy to run
```

### Hugo

```
$ brew info hugo
hugo: stable 0.80.0 (bottled), HEAD
Configurable static site generator
https://gohugo.io/
/usr/local/Cellar/hugo/0.80.0 (44 files, 82.3MB) *
  Poured from bottle on 2021-01-14 at 21:47:16
From: https://github.com/Homebrew/homebrew-core/blob/HEAD/Formula/hugo.rb
License: Apache-2.0
==> Dependencies
Build: go ✘
==> Options
--HEAD
	Install HEAD version
==> Caveats
Bash completion has been installed to:
  /usr/local/etc/bash_completion.d
==> Analytics
install: 17,994 (30 days), 46,735 (90 days), 253,979 (365 days)
install-on-request: 17,971 (30 days), 46,660 (90 days), 250,680 (365 days)
build-error: 0 (30 days)
```

- update hugo version https://gohugo.io/getting-started/installing/

```
# show the location of the hugo executable
$ which hugo
/usr/local/bin/hugo

# show the installed version
$ ls -l $( which hugo )
lrwxr-xr-x  1 mdhender admin  30 Mar 28 22:19 /usr/local/bin/hugo -> ../Cellar/hugo/0.13_1/bin/hugo

# verify that hugo runs correctly
$ hugo version
Hugo Static Site Generator v0.13 BuildDate: 2015-03-09T21:34:47-05:00
```

### Hugo Academic

On 2021/04/14, I updated `wowchemy` to [v5.0.0-beta.1](https://wowchemy.com/blog/v5.0.0-beta.1/) with the followings.
See [https://wowchemy.com/docs/](https://wowchemy.com/docs/) for more detail information.

```
hugo mod clean
hugo mod get github.com/wowchemy/wowchemy-hugo-modules/wowchemy@5fa9412
hugo mod get github.com/wowchemy/wowchemy-hugo-modules/netlify-cms-academic@5fa9412
hugo mod tidy
```
