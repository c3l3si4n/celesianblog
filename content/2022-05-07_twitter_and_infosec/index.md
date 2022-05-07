+++
title = "Twitter and Web Hacking"
+++

# Introduction

![Twitter](https://cdn.cms-twdigitalassets.com/content/dam/help-twitter/logos/htc-summary-card.jpg.twimg.768.jpg)


Twitter is a social media platform that works on publishing short texts and links on followers' feeds. Due to short texts usually being quick to write and share, Twitter is very good to use when you want to know about recent events early. This proves interesting when used in the Web Hacking world, where knowing early about the newest technique or vulnerability can reward you with a bug bounty on a reputable company or first blood in a CTF challenge.


# Tuning your feed
Your feed is mainly dictated by what content the people you follow post. This presents a common problem present with following many profiles in #infosectwitter which is that even though many of those profiles are managed by very reputable or technically-skilled people, the content they publish is often political off-topic nonsense or, cute but expendable animal pictures.

Off-topic tweets can seem harmless, but they act as noise that distracts you from the actual signal data. When there's much uninteresting content in your feed you will automatically zone out while scrolling and won't pay attention to an actual good article or vulnerability. 

Therefore, if a person you follow consistently posts off-topic content, consider muting that person on your feed so you still follow them while you don't see their posts.

The fewer people you follow, the less time you have to scroll Twitter to keep "updated" and the more you will learn per minute you spent reading Twitter. Twitter is best when used like an RSS news aggregator and not a proper friends social media platform.


# TweetDeck
![TweetDeck](https://i.postimg.cc/rm2JV0W8/tweetdeck.png)

Twitter's feed has multiple ways of being presented to the user. They are heavily pushing their feed algorithm to show you Unrelated Suggestions or Viral Tweets, which disturbs the experience you want to have with Twitter. Therefore, using alternate clients such as TweetDeck is preferable.

TweetDeck is an alternate Twitter frontend that offers a more convenient Twitter experience by letting you live-monitor multiple timelines or search queries in one easy interface.  This is possible by creating "columns" that update automatically whenever a new tweet matching that column filter is published on the platform, letting you know about new web hacking techniques seconds after it is shared first.

My current TweetDeck columns consist in the following:
- Home
- Activity (tweets people I follow liked or people they followed)
- Custom search filter to let me know about new unauthenticated vulnerabilities: `(preauth OR pre-authenticated OR preauthenticated OR pre-auth OR unauthenticated) AND (rce OR "code execution" OR "code injection" OR "SSRF" OR "XSS")`


# Conclusion

I hope that by reading this article, your Twitter experience will become better. Everything said here is purely anecdotal and is only based on my own experience using Twitter in the last 3 years. If you have any comments or suggestions for this article, please send me a DM on twitter.com/c3l3si4n . :)