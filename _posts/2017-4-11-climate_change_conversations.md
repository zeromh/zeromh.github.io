---
layout: post
title: Promoting Positive Climate Change Conversations via Twitter
thumbnail: /images/network_with_community_names.png
---

For my final project of the Metis Data Science program, I investigated the climate change conversations taking place on Twitter in March 2017. The 1 million tweets that I looked at were a snapshot in time - many users talked about EPA head Scott Pruitt's denial that CO2 causes global warming, or the critical condition of Australia's Great Barrier Reef. Sub-communities were also apparent within the greater conversation, including a group of climate change deniers who stood out from the rest.

<div class="image" align="center">
	<a href="/images/network_with_influencer_names.png" target="_blank">
        <img src="/images/network_with_influencer_names.png" alt="Most influential users"></a>
        <div style="font-style: italic; color: gray; font-size: 12pt">Twitter users who are the most influential in the climate change conversation (click to enlarge)</div>
</div>

But before I jump into the big picture results, I want to zoom in on one particular person...

<p align="center">
<img src="/images/mrlukeyluke.png" alt="Luke">
</p>

This is Luke. He's a British Twitter user. He's conservative, and he seems to communicate on Twitter mostly with other conservatives - many of whom deny the science of climate change. Luke, however, accepts the science. Which means that Luke, and people like him, potentially form a very valuable bridge between two groups - conservatives who deny climate change science, and liberals who accept it.

I'm going to tell you the story of how I found Luke.

The data I worked with for this project consisted of 1 million tweets from 560,000 Twitter users. This is a lot of unstructured data to deal with. But Twitter is a social network, which means it lends itself to network analysis. If you make a graph where each user is a "node," and you draw a line between nodes that communicate with each other - via retweets, replies, mentions, and so on - you get something that looks like this:

<div class="image" align="center">
	<img src="/images/initial_network.png" alt="Initial Network">
	<div style="font-style: italic; color: gray; font-size: 12pt">The network of Twitter users talking about climate change</div>
</div>

The layout of this graph is based entirely on who's connected to whom. Densely interconnected nodes will end up clustered together. One thing you might notice on this graph is that there is a group off to the right that looks like it's trying to get away from the rest of the users:

<p align="center">
<img src="/images/network_deniers_circled.png" alt="Climate change deniers">
</p>

And... that's basically true. If you read the tweets from that network, they overwhelmingly represent the climate change deniers:

<div style="color: gray; font-size: 11pt">
<ul>
        <li><b>@JunkScience</b>: An imperceptible change in mean global temp (a make-believe metric, anyway) hardly constitutes meaningful 'climate change'.</li>
        <li><b>@tan123</b>: Recent Research Shows Climate Models Are Mostly “Black Box” Fudging, Not Real Science</li>
        <li><b>@ClimateRealists</b>: Bad Weather Proves Climate Change, Says WMO....many are unable to see through this DECEPTION</li>
</ul>
</div>

The Deniers end up in their own cluster because they communicate more with each other than they do with the other groups. This goes to show that although the graph layout is based entirely on who communicates with whom, it also reflects deeper relationships - because humans choose who we communicate with based on who shares our beliefs and values.

I ran a community-finding algorithm on the network. The algorithm looks for nodes that are more interconnected than you would expect by chance. If we colorize based on those communities, we get this:

<p align="center">
<img src="/images/network_luke.png" alt="Network with Luke">
</p>

You can see Luke over there on the right. I'll come back to him in a minute.

I've given each community a name based on who it seems to represent. You can see individual Twitter users in the version at the top of this post. The size of the user name corresponds to their **influence** in the network. Influence is determined by the pagerank algorithm, which essentially looks at the quality and quantity of connections that each user has.

<div class="image" align="center">
        <img src="/images/network_with_community_names.png" alt="Network communities">
        <div style="font-style: italic; color: gray; font-size: 12pt">Different communities of the Twitter climate change conversation</div>
</div>

Once I had these communities, I wanted to know what they were talking about. I used latent dirichlet allocation (LDA) to discover the main topics in each community's conversation. Here are a few word clouds that describe some of the topics (with links to more information about each):

<div class="image" align="center">
        <img src="/images/pruitt_cloud.png" alt="Pruitt CO2 wordcloud">
        <div style="font-style: italic; color: gray; font-size: 12pt">
                <a href="https://www.nytimes.com/2017/03/09/us/politics/epa-scott-pruitt-global-warming.html">
                Scott Pruitt Denies that CO2 Causes Global Warming</a></div>
<br>
        <img src="/images/nye_cloud.png" alt="Bill Nye debate wordcloud">
        <div style="font-style: italic; color: gray; font-size: 12pt">
		<a href="http://insider.foxnews.com/2017/02/27/tucker-carlson-and-bill-nye-science-guy-clash-climate-change">
		Bill Nye and Tucker Carlson Debate Climate Change</a></div>
<br>
        <img src="/images/reef_cloud.png" alt="Great Barrier Reef wordcloud">
        <div style="font-style: italic; color: gray; font-size: 12pt">
		<a href="https://www.nytimes.com/2017/03/15/science/great-barrier-reef-coral-climate-change-dieoff.html">
		Large Sections of the Great Barrier Reef Are Dead</a></div>
<br>
        <img src="/images/inuit_cloud.png" alt="Inuit wordcloud">
        <div style="font-style: italic; color: gray; font-size: 12pt">
		<a href="https://t.co/GQLrsoEfS4">
		Create Archive of Inuit Knowledge to Help Adapt to Climate Change</a></div>
</div>

So you can see that each community had a unique set of topics that concerned them. Here's the list:

<div class="image" align="center">
        <img src="/images/community_topics.png" alt="Community topics">
        <div style="font-style: italic; color: gray; font-size: 12pt">Community Topics</div>
</div>

Some topics were shared by several communities, such as Scott Pruit's comments on CO2, or the argument between Bill Nye and Tucker Carlson on Fox News. I wanted to see if two communities could have different opinions on the **same** topic. So I used sentiment analysis to compare.

First I looked at the Scott Pruitt topic, since every community had talked about it. As you might expect, the climate change Deniers were incredibly positive about Pruitt's remarks, whereas everyone else was not:

<div class="image" align="center">
        <img src="/images/pruitt_bar.png" alt="Sentiment score for Scott Pruit comments">
</div>

I also looked at the Journalist community and the Denier community's discussion of Bill Nye. The two communities had very different sentiments, and if you read the tweets they disagreed completely about who won the argument:

<div class="image" align="center">
        <img src="/images/nye_bar.png" alt="Sentiment score for Bill Nye debate">
</div>

And this is how I found Luke. I realized that if I could determine differences in sentiment **between** communities, I might also be able to do it **within** communities. In other words, could I find individuals whose opinions differed from their communities at large?

Like I said, the Denier community wrote very negative things about Bill Nye. Here are some representative tweets:

<div style="color: gray; font-size: 11pt">
<ul>
	<li>Tucker Carlson OUTS Bill Nye as a Fake Science Guy 'Fraud' [VIDEO] via \n#weather #climatechange #hoax</li>
	<li>BOOM! Video: Tucker Carlson Eviscerates Bill Nye ‘The Nonsense Guy’ As To The ‘Settled Science’ of ‘Climate Change’</li>
	<li>BILL NYE’S EMBARRASSING FACE-OFF WITH TUCKER CARLSON ON CLIMATE CHANGE — It didn’t end well for 'Science Guy'…</li>
</ul>
</div>

Given how negative this community was on this topic, I looked for tweets that were much more **positive** than the average. This brought me to Luke's tweet on the subject:

<div class="image" align="center">
        <img src="/images/luke_tweet.png" alt="Luke's tweet about Bill Nye">
        <div style="font-style: italic; color: gray; font-size: 12pt">One of the most reasonable comments you'll find on social media</div>
</div>

Luke may not have the utmost faith in Bill Nye, but he agrees with Nye on climate change. This is in stark contrast with much of Luke's Twitter community. So it seems that Luke does form a bridge between conservative climate change deniers, and liberal acceptors. And I found a few other examples of “users who go against the grain” in other communities and topics that I looked at.

These users can be very important to find if you're doing outreach and advocacy. My analysis shows that, given tweets, we can find communities of users, their interests, and their sentiment. And we can see who an organization's Twitter audience is, who the Influencers are, and who forms important bridges between groups.

This was a really exciting project to work on. I learned a lot about network analysis (I didn't know anything when I started), and it's amazing how much insight I was able to find in this data once I'd structured it based on communities and community discussions. Thanks for reading!

\(Code for this project can be found at my Github [here](https://github.com/zeromh/proj5_climate_change)\)
