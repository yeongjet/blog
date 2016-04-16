+ In Picture View

`feh -g 640x480 -d -S *`

pervious or the next.

`p`, `n` use up or down arrows to scale.

`v` fullscreen or no fullscreen.

+ Base Usage

`-d` display file name

`-S` sort

`-F` view in fullscreen

-Z auto zoom

+ Wallpaper Setting

append below in your `~/.xinitrc`

`feh --bg-scale file.jpg`

recover the backgroud next time,also add

`` eval `cat ~/.fehbg` & ``

change wallpaper auto in random

```sh
#!/bin/bash
WALLPAPERS="$HOME/wallpapers"
while true; do
	find $WALLPAPERS -type f \( -name '*.jpg' -o -name '*.png' \) -print0 | \
	shuf -n1 -z | xargs -0 feh --bg-scale
	sleep 15m
done
```


+Advanced Usage

`feh -m -E 240 -y 320 -H 900 -W 1440 *`

`feh -c -E 64 -y 80 -H 900 -W 1440 *`

http://blog.chinaunix.net/uid-13745643-id-2873954.html
