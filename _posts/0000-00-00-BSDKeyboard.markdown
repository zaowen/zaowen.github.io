---
layout: post
title: "Of Mice,( and Keyboards) and MAN(1)"
date: 2017-08-13 00:00:00 -0600
categories:
---
```
I write code for a living, and by that I mean if it weren't for Stack Exchange I'd be homeless. 

--my brain
```

Which is better regarding understanding of a topic, good documentation or a good community?  My
first instict is to say documentation.  The persistance of shared knowlege excluseivly through oral
tradition is the reason there is a diffrence between [history and
prehistory](https://en.wikipedia.org/wiki/History_of_writing#Recorded_history).  However, this idea
lies in direct opposition to my first instinct when problem solving: 

```
10 Ask almighty google 
20 Click first stack exchange 
30 link Rephrase question 
40 goto 10.
```

I feel because of this the hardest problems I solve, at work or on personal projects, are the ones
that are hard to google. And not just because there have been occations when I have spent so much
time combing search engine results that I could have just figured the thing out. Instead I believe
it is because research is in it of itself a skill, and one I've been ignoring. I had recently
forgotten the exact syntax of a STL string function that I needed, and after finding a stack
overflow article on the subject I realized just a few results down the page was a
[cplusplus.com](cppreference.com) and another to [cppreference.com](cplusplus.com). What kind of a
person ignores the documentation for the forum? The kind of person who also thought it would be a
good topic for a blog post.

<hr>

I recently began trying out BSD.  FreeBSD makes a point of mentioning the "Quality and completeness
of their documentation" above and beyond that of the other distros. So I thought I would give it a
spin.  Trying to solve a problem with just man pages. That way no matter what happens I know how to
solve this problem. 

How do I change my keyboard layout to Dvorak? (Even though during the process of writing this I've
completely given up on the pipedream of avoiding repeditive strain injury).
Let's try the most obvious thing ever...

```
$ man keyboard
KEYBOARD(4)            FreeBSD Kernel Interfaces Manual            KEYBOARD(4)

NAME
     keyboard - pc keyboard interface
```

A quick skimming of the page let me know about scancodes and keymap stuctures, semi interesting as
far as general knowlege is concerned but it doesn't help. But then at the bottom I found this:

```
...

     The kbdcontrol(1) utility also allows changing these values at runtime.

AUTHORS
     Soren Schmidt <sos@FreeBSD.org>

FreeBSD 11.1-RELEASE            January 8, 1995           FreeBSD 11.1-RELEASE
```

Alright if the values can be changed at runtime, they should be able to be set by default and
kbdcontrol(1) is the next

```
$ man Kbdcontrol
KBDCONTROL(1)           FreeBSD General Commands Manual          KBDCONTROL(1)

NAME
     kbdcontrol - keyboard control and configuration utility

SYNOPSIS

...

     Keyboard options may be automatically configured at system boot time by
     setting variables in /etc/rc.conf.  See Boot Time Configuration below.

     The following command line options are supported:

```

Easy then.

```
KEYBOARD CONFIGURATION
   Boot Time Configuration
     You may set variables in /etc/rc.conf or /etc/rc.conf.local in order to
     configure the keyboard at boot time.  The following is the list of
     relevant variables.

     keymap       Specifies a keyboard map file for the -l option.
     keyrate      Sets the keyboard repeat rate for the -r option.
     keychange    Lists function key strings for the -f option.

     See rc.conf(5) for details.
```
```
     -l keymap_file
             Install keyboard map file from keymap_file.  You may load the
             keyboard map file from a menu-driven command, kbdmap(1).  The
             format of keyboard map files is documented in the kbdmap(5)
             manual page.
```

After looking at the man page for rc.conf I found that I need to add something 
<span title="rc.conf">[somewhere](#)</span> that looks like...

```
keymap="/usr/share/vt/keymaps/us.dvorak.kbd"
```

and that did it. After adding that line to my rc.conf and a reboot and it was official all my keys 
were in dumb places just like I wanted it. Or so I thought...

After booting into my Desktop Environment my keymap was back to normal. That's disheartening.
However with no more leads the best thing to do is keep looking at the ones you have.  While looking
through kbdmap manpages I found this amazing statement.

```
$ man kbdmap
KBDMAP(1)               FreeBSD General Commands Manual              KBDMAP(1)

...

BUGS
     The kbdmap and vidfont utilities work only on a (virtual) console and not
     with X11.
```

One thing I really like about the BSD man pages is things like this. Important ittle informational
links that connect you to the next piece of the puzzle.

```
$ man X
X(7)               FreeBSD Miscellaneous Information Manual               X(7)

NAME
       X - a portable, network-transparent window system
...

SEE ALSO
       xterm(1), xwd(1), xwininfo(1), xwud(1).  Xserver(1), Xorg(1), Xdmx(1),


$ man Xorg
Xorg(1)                 FreeBSD General Commands Manual                Xorg(1)

NAME
       Xorg - X11R7 X server

...

OPTIONS
       Xorg supports several mechanisms for supplying/obtaining configuration
       and run-time parameters: command line options, environment variables,
       the xorg.conf(5)


$ man xorg.conf
xorg.conf(5)              FreeBSD File Formats Manual             xorg.conf(5)

NAME
       xorg.conf, xorg.conf.d - configuration files for Xorg X server
...
       The second type of entry is used to match device types. These entries
       take a boolean argument similar to Option entries.

       MatchIsKeyboard     "bool"
...
       Input drivers: kbd(4) among others...


$ man kbd
KBD(4x)                                                                KBD(4x)

NAME
       kbd - Keyboard input driver
...
       Option "XkbRules" "rules"
              specifies which XKB rules file to use for interpreting the
              XkbModel, XkbLayout, XkbVariant, and XkbOptions settings.
              Default: "base" for most platforms.  If you use the "base" value
              then you can find listing of all valid values for these four
              options in the /usr/local/share/X11/xkb/rules/base.lst file.

...
EXAMPLE
           Section "InputDevice"
               Identifier   "Generic Keyboard"
               Driver       "kbd"
               Option       "CoreKeyboard"
               Option       "XkbRules"      "base"
               Option       "XkbModel"      "pc105"
               Option       "XkbLayout"     "us,sk"
               Option       "XkbVariant"    ",qwerty"
               Option       "XkbOptions"    "grp:menu_toggle,grp_led:scroll"
           EndSection
```

With all the information gained altering the example from the kbd page into what I need was not too
difficult.  And instead of simply obtaining the config, its meaning is built upon a foundation that
itself can be built upon. if that sort of thing means anything to you.

Who knows thought, I think its important.

```
Section "InputClass"
        Identifier "keyboard"
        MatchIsKeyboard "on"
        Option "XkbRules" "base"
        Option "XkbLayout" "us"
        Option "XkbVariant" "dvorak"
        Option "XkbModel" "pc105"
EndSection
```
