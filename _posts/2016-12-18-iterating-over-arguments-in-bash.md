---
layout: post
title:  "Iterating Over Arguments in Bash"
date:   2016-12-18
tags: [bash]
---
Arguments can be accessed using `$@`. In contrast to `$*`, `$@` correctly parses quoted arguments. Given the following script name `args.sh`:

```bash
for var in "$@"
do
  echo "$var"
done
```

Running this script with the following arguments yields:

```bash
sh args.sh a b "c d"
a
b
c d
```

Pretty useful for scraping stuff, creating conditional directories, renaming files, etc...