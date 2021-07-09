# The Machinery book

This repository contains the source of "The Machinery" book and some other books. This project is a work in progress!

## How to contribute?

If you want to contribute to this project please read the [CONTRIBUTING.md](CONTRIBUTING.md) first!

## Requirements

Building the book requires [mdBook](https://github.com/rust-lang-nursery/mdBook). To get it:

```
$ cargo install mdbook
```

Alternatively you can download a latest executable from this [link](https://github.com/rust-lang/mdBook/releases/).

## Writing

Open the markdown files with the editor of your choice. If you want to see the changes live run:

```bash
# for the introduction
$ cd the_machinery_book
# otherwiese tutorials to open the tutorial book
$ cd tutorials
$ mdbook serve
```

## Building

To build the book, type:

```bash
$ # for the introduction
$ cd the_machinery_book
$ # otherwiese tutorials to open the tutorial book
$ cd tutorials
$ mdbook buil
```

The output will be in the `book` subdirectory. To check it out, open it in your web browser.

*Firefox:*

```bash
$ firefox book/index.html                       # Linux
$ open -a "Firefox" book/index.html             # OS X
$ Start-Process "firefox.exe" .\book\index.html # Windows (PowerShell)
$ start firefox.exe .\book\index.html           # Windows (Cmd)
```

*Chrome:*

```bash
$ google-chrome book/index.html                 # Linux
$ open -a "Google Chrome" book/index.html       # OS X
$ Start-Process "chrome.exe" .\book\index.html  # Windows (PowerShell)
$ start chrome.exe .\book\index.html            # Windows (Cmd)
```
