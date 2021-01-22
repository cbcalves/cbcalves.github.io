+++
author = "Carlos Alves"
title = "Konsole Re-Flow Lines"
date = "2020-01-23"
description = "Coding the Konsole Re-Flow Lines"
tags = [
    "KDE",
    "Konsole",
    "Programming",
    "KDE Plasma",
    "Open Source"
]
+++
### The Re-flow
One day I was looking at the MR (Merge Request) and saw [Tomaz Canabrava's](https://tcanabrava.github.io/) sketch showing the [terminal re-flow lines while it shrinks](https://invent.kde.org/utilities/konsole/-/merge_requests/181), and I just thought it would be great to have it fully working.

### First try
The first thing to have a line re-flow is define how to mark a line with "continues in the next line". This is the most important thing, otherwise you can't go back to original state. My first thought was to set a next line char with something not printable, and then the first screen re-flow on both ways prototype was done.

To improve speed, and hold lines before send to memory, the next thing I did was change the _screenLines holder from an array to a QVector type. It was an improvement in speed, specially to re-flow. No need to a new memory allocation, no copy and no delete, it was just update the QVector content and send from the QVector to history, if needed, and resize it.

While studding how to re-flow the history I discovered there was a property (maybe from an old try to re-flow, but used by some terminal programs) to mark a line as "continue in next line" the: "LINE_WRAPPED", I removed the "new line char" and changed the code to use this property.

After some bug fixes, the screen lines were being re-flowed again with this new property, and I could start to work with the fixed size history re-flow, to have it done first I wrote how it would re-flow and what auxiliary functions I would need to manipulate history before actually code the fixed size history re-flow, needed functions like update positions, insert positions and remove positions.

As the fixed size history started to re-flow, and lots of bugs fixed, I started to fix some glitches like the scroll bar and the screen position going crazy while re-flowing. Doing tests I found that when changing the screen size while in an app, the screen was not re-flowed after quit the app, it happened because the app changes the cursor line from a screen that it is not using, so it had now to get the saved state when it was in an app.

With all this done I thought it was ready to go: weeks of tests and no new bugs. It was merged to master and reverted in a few hours because of a bug in unlimited history.

### Merged
Back to the [re-flow project](https://invent.kde.org/utilities/konsole/-/merge_requests/321), bug fixed, added some new unlimited history manipulation to update the last line and delete the last line. As it seems to me it was impossible to re-flow the unlimited history as it is unlimited, well with some people it can be something near to an infinite loop.

While fixing the re-flow, [Nate Graham](https://pointieststick.com/) came to the MR and asked if it was going to be optional, because some people didn't want to use it. As I was working with something that I considered to be really cool in Konsole, I never thought about this, that someone could not want to use it, but they are right, sometimes we might be working on something and not want to have it re-flowing. I added an option to set the re-flow off in the profile.

And the last commits were about performance, I was thinking about the re-flow performance since I had a video call with Thomas Canabrava, where he showed me the unlimited history bug. But I noticed how it was using my processor to re-flow two tabs and I wanted it to do better. So I reordered the data flow and re-factored some functions to improve the re-flow performance, reducing the number of loops and simplifying the data flow.

### Unlimited
It was time to think on the [unlimited scroll re-flow](https://invent.kde.org/utilities/konsole/-/merge_requests/330), as it have a different way to save the lines from the fixed size, I realized it was going to need a different way to handle it, as fixed size and unlimited scroll have very different methods to save the lines.

The unlimited scroll is saved in three files: one for the characters, one for the indexes and one for the 'next line' mark. While the fixed size history is just one vector. 

I choose to change just the indexes and 'next line' mark, it was not necessary to change the characters content.

First step was move the fixed size history code to CompactHistoryScroll and set the screen to call this function when it was needed. Then I started to code the unlimited re-flow in the HistoryFileScroll to be called with the same command.

And it worked, it does the re-flow just changing the indexes of begin, end of lines and setting the 'next line' marks. After setting a limit (the infinite loop), with some tests, to 20,000 lines the MR is up.

### Testes
After writing some testes for both re-flows codes, and fix some bugs found with the testes, it is finally ready.