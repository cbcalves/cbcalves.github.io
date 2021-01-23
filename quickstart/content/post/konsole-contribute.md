+++
author = "Carlos Alves"
title = "Contributing to Konsole"
date = "2021-01-24"
description = "How I started contribute with Open Source and Konsole"
tags = [
    "KDE",
    "Konsole",
    "Programming",
    "KDE Plasma",
    "Open Source"
]
+++
### Discovering
I never thought I could contribute with Open Source, or even imagined I could change my workspace, in my mind doing it was beyond my programming skills.

I was a Windows user for a long time, until one day I couldn't stand anymore how the system was so slow, it was not a top computer, but it was a reasonable one to be that slow.

So I changed to Debian and used it for a time until change to other distros, but I was amazed how fast it was, of course I couldn't use all of the same programs I used to work with but I did learn new ones.

### The Open Source Contribution
Time goes on and last year (2020), with the pandemics, I've got some spare time at home, so I decided to learn new things, I solved problems and challenges in different programming languages, and discovered [Qt](https://www.qt.io/), well Qt was so connected to [KDE](https://kde.org/) that somehow I ended up watching lives about "How to contribute with Open Source" (2020 was the year of "lives"?) and events that talked about it.

I was using Qt with some private things and thought that I should see how this Open Source contribution really works, my first choice was the program that I'm always using: the terminal (in [KDE Plasma](https://kde.org/plasma-desktop/): [Konsole](https://konsole.kde.org/)), probably 99% of linux/unix users have a terminal opened.

### Konsole
After downloading the code, some time installing dependencies (I guess everyone new to it have a problem here, lets skip it) I finally got it compiled and running. Than reading enough of pages, codes and testing some fixes, I lost the fear of not being good enough and submitted the fixes.

I started with small bugs. Than increased the difficult level. And finally be able to add a new feature.

The most difficult to fix was a crash that happened when rearranging split view, because it was a clean crash, I debugged it a lot (used gdb, gammaray, etc), there was no memory leak, no code error, no access problem, nothing. Than I started to analyze all event calls that could be triggered and realized it was connecting all terminals into an object marked for deletion with a "deletelater" call, and this was creating a chain reaction deleting everything in the tab, with no errors, this fix was very small actually but the process was very time consuming.

And of course there is the "fix" that will haunt me for a few more months: the intense colors mess up. Sometimes we do mistakes codding, revert it, and live goes on, but not for me, bugs report about this will keep coming just to remind me.