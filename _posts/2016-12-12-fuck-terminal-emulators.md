---
layout: post
title:  "Fuck Terminal Emulators"
date:   2016-12-12 02:54:18 -0700
categories: tech horribleness terminal terminal_emulators
---

# Overview of terminal emulators and brokenness

This is a brief overview of my hunt for a pretty, not-buggy, nicely functional terminal setup. I'm not even going to touch on configuration of shells/editors here, because that's a whole other basket (which makes this worse, but.

# What I'm looking for

I'm not looking for too much, I thought, but apparently not.

* I'm looking for good font support. Not having Powerline characters be a pixel off in an axis or two. Being able to use Powerline characters and FontAwesome characters and such and have them display.
* I want it to work. I don't want to spend 20 hours per tweak testing and googling and reading the source and changing it and recompiling.
* Good theme support would be _nice_. Especially if I can have it hooked up to things like vim and have vim change colors so it looks okay (even though, yes, this would probably be a lot of work on my end, but bonus points).

# Overview of different emulators I've looked at

## libvte-based emulators

A variety of emulators are based on a widget provided by `libvte`, like `gnome-terminal` or `lxterminal` or `sakura` or `evilvte`. There's no shortage.

### Problems:
* Only pick a single font, seems to fallback by fontconfig.
* Colorscheme support is usually bad.
* Powerline symbols are off by < pixel, but very visible.

## XTerm

One of the oldest emulators out there. Included for completeness.

### Problems:
* Horrible font/Unicode support.
* xresources for settings.
* In general, one of the worst options.

## rxvt (specifically, rxvt-unicode)

Fairly popular emulator. Pretty extensible, nice font support when you can understand it.

### Problems:
* Settings are xresources
* Need an obscure patch that asks the font how wide chars are to render a lot of chars properly.
* Powerline off by a pixel.

## Hyper (Hyper.app)

Fancy, web based terminal. HTML/CSS/JS galore, extensions in JS as NPM packages, etc.

### Problems:
* Linux clearly second class to OSX, much is worse
* No key bindings seem to work, many plugins don't seem to handle linux well, etc
* Font fallback is great, but Powerline chars are, again, a pixel off. Still.

## Terminology

Fancy enlightenment terminal emulator. Can cat images into it, display videos, ls with thumbnails, etc.

### Problems:
* Lacks true color support.
* Uses fontconfig, again.
* Powerline symbols are the most off to me, visually.

## Black Screen

Another web technologies emulator. Only officially supports OSX currently, but it runs on linux. Super fancy UI.

### Problems:
* UI does not play the best with `sudo`-ing to other users.
* Not very configurable at all, currently. No config, no themes, etc.
* Powerline chars are super off vertically, hard to tell if others are.
* Actually displays all the nonstandard chars though, probably thanks to my fontconfig?
* All in all, very _very_ **VERY** experimental. Cool, but definitely not ready for me to use even if I wanted.

# Summary:

Fuck terminal emulators.

# Notes:

Test true colors with ```awk 'BEGIN{
    s="/\\/\\/\\/\\/\\"; s=s s s s s s s s;
    for (colnum = 0; colnum<77; colnum++) {
        r = 255-(colnum*255/76);
        g = (colnum*510/76);
        b = (colnum*255/76);
        if (g>255) g = 510-g;
        printf "\033[48;2;%d;%d;%dm", r,g,b;
        printf "\033[38;2;%d;%d;%dm", 255-r,255-g,255-b;
        printf "%s\033[0m", substr(s,colnum+1,1);
    }
    printf "\n";
}'```.
I'm testing the nonstandard chars by `sudo`-ing to a different user that has a prompt like ```  mist@collar   ~                                767  02:30:49 ```, though with colors too. You'll need to have, I believe, something like powerline fonts + octicons + probably more to see all of that right. The full list of fonts I want to fall back on is something like `"Source Code Pro", PowerlineSymbols, Pomodoro, FontAwesome, Octicons, Icomoon, "Noto Color Emoji", "Source Han Sans JP"` (also `unifont` but the only place I actually have that implemented is in `urxvt` and not via `xft` so I don't know the font name).
