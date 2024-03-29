---
title: R. O. N. DataBase Manager
date: 2023-03-31T13:32:00-04:00
description: Rusty Object Notation as a database to replace XRDB
slug: ron-db
categories:
    - Xorg
tags:
    - Rust
weight: 1
---

<!-- LTeX: language=en,es -->
**rdbm** is a rust program for getting and setting a custom ron resource file.

Similar to `.Xresources`, `rdbm` will create a `resources.ron` in your `~/.config/` directory.
You can then use `rdbm` to set multiple "key-value" pairs similar to `.Xresources`.
In this case `rdbm` aims to replace `xrdb`.

- [ron Documentation](https://github.com/ron-rs/ron)
- [xrdb Information](https://wikipedia.org/wiki/Xrdb)

# Documentation

Here is a short preview of each subcommand:

## view current resource file

```
rdbm all
```

## set a key-value pair

```
rdbm set "color0" "#222F30"
```

## get a value

```
rdbm get "color0"
```

## rdbm help page
```
rdbm --help
rdbm help
rdbm set --help
```

# Project Goals
 * Remove entries
 * Get/Set multiple values
 * Custom `resource.ron` path
 * Group key-value pairs (similar to having `URxvt*color0` and `xterm*color0` in .Xresources)
 * Sort entries in resource file
 * Preserve order of entries in resource file

# Links
 * Link to github repo: https://github.com/Sunglas/rdbm
