# weblink

## Description

A CLI program for generating and converting clickable URL files.

Each platform has its own file format for clickable files for opening URLs:

* `.desktop` (Linux)
* `.url` (Windows)
* `.webloc` (macOS)

`weblink` can generate and convert between all three.


## Installation

### Pre-requisites

* Ruby

### via git

```shell
  git clone https://github.com/thoran/weblink
  cp weblink/bin/weblink $PATH
```

### via Homebrew

```shell
$ brew tap thoran/tap
$ brew install thoran/tap/weblink
```


## Usage

The general form is:

```
weblink [g(en(erate))|c(on(vert))] [--format FORMAT] [--output PATH] [URL_OR_PATH]
```

The `generate` and `convert` sub-commands can be abbreviated. Input can be supplied as an argument or piped in on STDIN. Output goes to STDOUT by default, or to a file when `--output` is given.

### Generate

The `generate` sub-command is optional. The format defaults to whatever is native to the current OS, but can be overridden with `--format` or inferred from the extension of `--output`.

```shell
$ weblink https://example.com # inferred format (What OS are we on?) to STDOUT
$ weblink generate https://example.com --format url # explicit to STDOUT
$ weblink https://example.com -o ~/Desktop/Example.webloc # inferred format to a file
$ echo https://example.com | weblink --format desktop # via STDIN
```

### Convert

The `convert` sub-command is required when converting. It will convert an existing shortcut file to another format. The source format is detected from the file extension; the target comes from `--format` or from the `--output` extension.

```shell
$ weblink convert /path/to/example.webloc --format url # explicit format to STDOUT
$ weblink convert /path/to/example.url -o example.desktop # inferred format to file
$ cat /path/to/example.webloc | weblink convert --format desktop # via STDIN
```


## As a Library

It can also be used as a library.

### Generate

```ruby
web_link = WebLink.new('https://example.com')

web_link.to_desktop
# => #<DotDesktop:>
web_link.to_url
# => #<DotUrl:>
web_link.to_webloc
# => #<DotWebloc:>

web_link.to_webloc.generate
# => #<String:>
web_link.to_url.generate
# => #<String:>
web_link.to_desktop.generate
# => #<String:>
```

### Convert

```ruby
file = File.read('/path/to/example.desktop')
web_link = DotDesktop.parse(file, 'desktop') # explicit format
# or
web_link = WebLink.load('/path/to/example.desktop') # dispatch by file extension
# => #<DotDesktop:>

file = File.read('/path/to/example.url')
web_link = DotUrl.parse(file, 'url') # explicit format
# or
web_link = WebLink.load('/path/to/example.url') # dispatch by file extension
# => #<DotUrl:>

file = File.read('/path/to/example.webloc')
web_link = DotWebloc.parse(file, 'webloc') # explicit format
# or
web_link = WebLink.load('/path/to/example.webloc') # dispatch by file extension
# => #<DotWebloc:>

web_link.generate
# => #<String:>
```


## Contributing

1. Fork it: `https://github.com/thoran/weblink/fork`
2. Create your feature branch: `git checkout -b my-new-feature`
3. Commit your changes: `git commit -am 'Add some feature'`
4. Push to the branch: `git push origin my-new-feature`
5. Create a new pull request


## Dependencies

- [switches.rb](https://github.com/thoran/switches.rb)


## History

`weblink` started life as `weblocator`, a `.webloc`-only tool, and was generalised to cover all three formats. See the `CHANGELOG` for details.


## Licence

MIT
