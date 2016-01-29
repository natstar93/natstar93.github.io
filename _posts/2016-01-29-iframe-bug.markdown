---
layout: post
title:  "iframe iPhone 6 bugs and quirks"
date:   2016-01-29 17:12:28
categories: Node
image:
image-subtitle:
---

A lot of my time over the past couple of days has been spent trying to work out why a <a href='https://www.doubleclickbygoogle.com/solutions/revenue-management/' target="_blank">Google DFP banner</a> was rendering perfectly and responsively on desktop browsers and most mobile phone devices but not on the iPhone 6 range.

DFP banners are normally used to display banner adverts, but at Trainline we use them to show messages relating to service disruptions e.g. 'Tube strike on 4/5/16'.

Here is one of those banners on an iPhone 6S. It thinks it's a desktop banner and is trying to escape off the side off the screen :(

<img src='http://www.natalie-akam.com/images/disruption.png'/>

The problem was with the <iframe> element which wraps my banner code and is inserted by DFP. Unfortunately just telling it to have a width of 100% doesn't cut it for iPhone 6. Safari will allow the content to stretch and overflow outside the actual width of the screen. It took a while to diagnose the problem and find a workaround, but the below CSS hack fixed it:

    iframe {
      width: 1px;
      min-width: 100%;
    }

Here the iframe width is set to 1px then overridden. This allows the iframe to achieve a lovely responsive 100% width on everthing. Happy days!

Nat x
