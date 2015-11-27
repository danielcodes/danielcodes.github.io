---
layout: page
title: Portfolio 
---

## Pipeline Monitor
(Ongoing) A pipeline monitor in essense tells you whether the input at one stage has successfully passed to another. This concept exists in UNIX where several commands can be concatenated with the | symbol. *ie. grep keyword file.txt | less*, this simple pipeline uses grep to look for a keyword in file.txt and then displays the output with less (scroll). Several things can go wrong here, no occurrences of the keyword, the file may not exist, etc.

We (Team and I), decided to apply this concept to a web application. The process we build was a news aggregator. We collected information about articles through rss feeds and displayed them on a page. A sub process was also created to follow the article's link get the images relating to that article.

See it here, [pipeline](http://danielchia.me) and [news](http://danielchia.me/news)

