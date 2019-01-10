---
layout: post
title:  A couple neat vim commands
date:   2019-01-10
tags: [vim]
---
### Managing vim buffers

Being a vim noob, I first painfully cycled through buffers using
filename autocomplete to open up the one I needed. Although I have tmux running,
I don't spend as much time jumping back and forth between tmux
windows/panes to simultaneously tackle different projects anymore. So
now I can generally stay in the same vim window and use splits pretty
effectively.

After an entire day of opening files, it's common to end up with tens of
different buffers and navigating them started to become a pain. Science
be praised for there are tools to keep buffers under control. **:bd**
can be used to delete a buffer, minimizing the buffer noise when I'm
done with a file.

Another neat command is **:ls**. This lists open buffers and associates
them with an ID. The ID can be used to quickly open up the corresponding
buffer, e.g., `:b 1` opens up the buffer with ID 1.

```
:ls
  1 %a   "_posts/2019-01-10-a_couple_neat_vim_commands.md" line 20
  Press ENTER or type command to continue
```

### More convenient control over sizing panes

Forgot where I stole this from, but I mapped some quick height/width
pane resizing to my leader key. When I have a bunch of panes open and
want to quickly focus on one particular pane, I use ZoomWin (bundled as
part of the [Janus vim distro](https://github.com/carlhuda/janus)).

```
nnoremap <silent> <leader>+ :exe "resize " . (winheight(0) * 3/2)<CR>
nnoremap <silent> <leader>- :exe "resize " . (winheight(0) * 2/3)<CR>
nnoremap <silent> <leader>> :exe "vertical resize " . (winwidth(0) * 3/2)<CR>
nnoremap <silent> <leader>< :exe "vertical resize " . (winwidth(0) * 2/3)<CR>
```
