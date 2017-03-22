---
layout: post
title:  "Unix Permissions"
date:   2016-12-11
tags: [unix]
---
Unix permissions are represented with symbolic notation and octal notation. It has four parts:

* file type
* class permissions
  * **user**
  * **group**
  * **others**

### Symbolic notation
* r : read
* w : write
* x : execute
* - : placeholder for when corresponding permission is omitted

### Octal notation
* 4 : read
* 2 : write
* 1 : execute

```
| Symbolic    | Octal   | Description                                                                 |
|------------ |-------  |---------------------------------------------------------------------------  |
| ----------  | 0       | no permissions                                                              |
| -rwx------  | 700     | read, write, execute for owner                                              |
| -rwxrwx---  | 770     | read, write, execute for owner and group                                    |
| ---x--x--x  | 111     | execute for owner, group, others                                            |
| --w--w--w-  | 222     | write for owner, group, others                                              |
| -r-xr-xr-x  | 555     | read and execute for owner, group, others                                   |
| -rwxr-----  | 740     | read, write, execute for owner; read for group; no permissions for others   |
```