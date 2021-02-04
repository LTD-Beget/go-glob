# String globbing in golang

`go-glob` is a single-function library implementing basic string glob support.

Globs are an extremely user-friendly way of supporting string matching without
requiring knowledge of regular expressions or Go's particular regex engine. Most
people understand that if you put a `*` character somewhere in a string, it is
treated as a wildcard. Surprisingly, this functionality isn't found in Go's
standard library, except for `path.Match`, which is intended to be used while
comparing paths (not arbitrary strings), and contains specialized logic for this
use case. A better solution might be a POSIX basic (non-ERE) regular expression
engine for Go, which doesn't exist currently.

## Why does this fork exist?

If we want to do string matching against the same pattern multiple times,
for performance reasons, we don't need to prepare this pattern for each iteration.
It's enough to do it once.

Looks like [PR](https://github.com/ryanuber/go-glob/pull/7) to the main repo hung, so I decided to just release a new module.

Example
=======

```
package main

import "github.com/vasayxtx/go-glob"

func main() {
    // Glob
    glob.Glob("*World!", "Hello, World!") // true
    glob.Glob("Hello,*", "Hello, World!") // true
    glob.Glob("*ello,*", "Hello, World!") // true
    glob.Glob("World!", "Hello, World!")  // false
    glob.Glob("/home/*", "/home/ryanuber/.bashrc") // true
    
    // Compile
    helloGlob := glob.Compile("Hello,*")
    helloGlob("Hello, World!") // true
    helloGlob("Hi, World!")    // false
}
```
