---
layout: post
title: Tips for Data Analysis: An Example Using MTA Turnstile Data
---
## The Problem
The hardest part of an open-ended data science project is often defining concretely what your goal is.

For example, let’s say you’re tasked with finding the best subway stations in New York City for a non-profit organization to send canvassers to (the canvassers will solicit email addresses and/or donations.) How do you do this?

Perhaps you want to find stations with lots of traffic, where the subway riders are interested in the NPO’s mission. This is simple enough, but how do you deal with tradeoffs between these factors - such as stations with middling traffic, but high interest in the cause? Or stations with high traffic, but where the ridership has a low income for making donations? You’ve been tasked with finding the “best” subway stations, but the hard part is defining what “best” means.

Here are some thoughts on how you can do that, for any analytics project.

My first project at Metis Data Science Bootcamp was very similar to the above problem. I worked with a team using passenger entrance/exit data from the New York subway system to determine the optimal placement of street canvassers. There were a lot of factors we considered using in our selection process:
- volume of foot traffic into/out of the subway stations
- income of residents living near each station
- station proximity to universities
- station proximity to tech hubs in NYC (this was relevant to the mission of the NPO, which was to increase representation of women in tech)
- passenger responsiveness to solicitation (probably low during rush hour, higher at other times)

Ultimately what we need is a list of the top 15 or so stations to send canvassers to. This means we need to rank each station. But how do we determine a rank from all of the variables that we want to consider? As mentioned above, how do we deal with tradeoffs between, say traffic and income?

## A Solution
One approach is to write simple equations that describe what variables affect your outcomes. This will be somewhat theoretical and you will have to make some assumptions, but the method is useful even if it isn’t perfect.
 
Let’s make a reasonable assumption: the number of people a street team signs up in a day is roughly equal to the number of people they talk to in an hour, times the percentage of people who are interested in the cause enough to sign up, times the number of hours the street team is out working. In other words:
S (signups) = T (number talked to per hour) * I (percent interested) * H (hours)
200 people per hour * 3% are interested * 6 hours = 36 people signed up

If we assume that 1) H will be set by the NPO's budget, and 2) our subway stations will have at least 200 people passing through per hour (an easy minimum to meet in NYC), then the only variable we can influence in this equation is I (interest), which we can do by targeting parts of the city where subway riders will be more interested in supporting women in tech.

From our equation we can also see that if we double the percentage of interested riders, we will double the number of signups! So “interest in the cause” is a factor we should definitely focus on.

## Donations
So let’s say we’ve gained lots of email subscribers. At some point, the NPO will send out emails asking for donations to their cause. Only a certain percentage of those emailed will donate. This percentage is often called a conversion rate, and the simple equation to describe it is:

D (number of donations) = S (signups) * C (conversion rate)

The NPO will be able to affect the conversion rate via the way they handle their email list and keep their subscribers happy. But from our perspective, C is fairly set. As consulting data scientists, we can only increase the number of donations by increasing S, the number of email signups. We talked about that in the previous section.

That said, with the proper subway station recommendations we can probably affect the average **dollar amount** of each donation received.

The reason we thought to consider income above was because we thought that it might be correlated with the amount that someone donates. This assumption may not be true, but let's explore where it takes us. If we assume that a person might give a donation equaling .2% of their annual income, then it becomes clear that, all else being equal, sending street teams to talk to subway riders with high incomes will effect larger donations:

A (amount donated) = G (gross annual income) * .2% * D (number of donations)

Again, this is predicated on the assumption that income is correlated with donation size. For a serious project this is something that we would have to verify. But given that assumption we can see that donation size is directly proportional to income, and so that makes income a another factor we should focus on in our analysis.

Combining our equations for Signups, Donations, and Amount we see that:

A = .2% * G (gross income) * T (number talked to/hour) * I (percent interested) * H (hours) * C (conversion rate)

Now we can easily answer questions like “how good is a subway station where riders have high interest in the cause but low income?” For two stations with average rider income of $45k and $80k, and with percentage of riders interested in the cause at 6% and 3%, respectively:

A = .2% * $45000 * 200 * 6% * 6 hours * 1% conversion rate = $86.20 (and 72 signups)
A = .2% * $80000 * 200 * 3% * 6 hours * 1% conversion rate = $57.60 (and 36 signups)

Despite the first group having much lower average income, we get more donation money and more email signups from that group because of their high level of interest in the NPO’s cause. In fact, it’s obvious from our equation that if Group A has double the interest of Group B, we will get more signups and donations from them unless their income is fully **half** that of Group B.

::mention traffic amount and time of day::
