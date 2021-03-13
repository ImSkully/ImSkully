---
title: "Tabs vs. Spaces"
---

I've always preferred tabs over spaces, four spaces to be exact but unlike the fact that the number of spaces doesn't really make a difference in indentiation other than just how it looks, choosing to use spaces over tabs is ridiculous.

## Pro Tabs
Personally it has never been a choice resulting from visual aesthetics but rather performance and having an optimized work flow. We indent for the sake of making code readable, and that is the sole purpose of it - when it goes through the compiler, it won't matter. *(unless you're an anal programming language by the name of Python)* Working with tabs has always been easier for the sole fact that when you want to move around the file, moving up and down lines, or adding/removing indentation, you are **moving through a whole block of indentation rather than one block at a time.**

A block is either one tab character, or one space character. The benefit of tabs is that a single tab character can consist of any number of spaces, whereas a space character alone is a block character of its own; they cannot be grouped up, they are separate and when you want to move horizontally across a file using an arrow key, you have to pass through each space character within your indentation however when it comes to tabs, you simply jump across to the end of the tab block and boom, you're exactly where you want to be.

Sure, it's arguable that it comes down to preference at the end of the day because there are people out there who move across files using their mouse for navigation rather than keybinds *(an argument for another day)* but one thing that cannot be denied here by space character users is **file sizes.**

A tab character is counted as a single ASCII character whereas having four spaces are counted individually and make four separate characters, therefore taking up more bytes. If you want to argue that the number of bytes here is insignificant to ever actually matter, than you clearly haven't worked on a project large enough with thousands of files, or you truly don't appreciate the importance of file compression and how much it matters to the likes of web developers where saving every byte over the network counts - file size will always matter at the end of the day and using tabs is one way developers counter this - a perfect example of this which comes to mind is when recently I had the the case where I couldn't .gzip a file or minify it therefore converting all of the indentation from spaces to tabs made an actual difference in file size.

## But why spaces?
I still don't understand why any concious being would willingly choose to pick spaces over tabs. You're choosing to work slower, you have more clicks to conduct to move around the file, you have more buttons to press to indent and you're definitely annoying those who work with you by slapping the spacebar multiple times instead of pressing one tab. You are adding unnecessary bytes to files which I will then have to download locally when I visit your website or use your application and over a period of days, months, years multiplied by average visits, the number of bytes adds up.

## Who cares?
Granted nowadays with indentation automatically being handled for us by IDEs and work flow plugins such as linters, it doesn't really matter. Indentation converts automatically when you save a file with your preferences set, spaces turn to tabs, or vice versa. The only thing that still affects me is when I go to download some code off GitHub and don't plan on actually working with it, but just want to view it or fork the code - sometimes letting the IDE handle automatic conversion doesn't work, other times we choose not to use it; I dislike linters, they get things wrong and are never correct, sometimes I'm just indenting my code and formatting a comment or some inline documentation to look a specific way for readability, or trying to format a table so its more easily understood and to this day I have yet to see a linter that takes this into consideration and unfortunately at the end of it all, I prefer to manually execute the command to convert indentation rather than trusting automation to handle it.

## tl;dr
If you press spacebar eight times instead of tab, you're a freak.