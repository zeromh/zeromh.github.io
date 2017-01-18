---
layout: post
title: Data Science - Defining a Goal
---
The hardest part of an open-ended data science project is often defining concretely what your goal is.

For example, let’s say a non-profit organization hires you to find the best subway stations in New York City to send canvassers to (the canvassers will solicit email addresses and/or donations for the organization.) How do you do this?

Perhaps you want to find stations with lots of traffic, where the subway riders are interested in the NPO’s mission. This is simple enough, but how do you deal with tradeoffs between these factors - such as stations with middling traffic, but where the riders have high interest in the mission? Or stations with high traffic, but where the ridership has a low income for making donations? You’ve been tasked with finding the “best” subway stations, but the hard part is defining what “best” means.

Here is a way to define a concrete, useful goal for almost any data science project.

## So Many Variables!
My first project at Metis Data Science Bootcamp was very similar to the above problem. I worked with a team using [passenger entrance/exit data] (http://web.mta.info/developers/turnstile.html) from the New York subway system to determine the optimal placement of canvassers. There were a lot of factors we considered using in our selection process:


- volume of foot traffic into/out of the subway stations
- income of residents living near each station
- station proximity to universities
- station proximity to tech hubs in NYC (this was relevant to the mission of the NPO, which was to increase representation of women in tech)
- passenger responsiveness to solicitation (probably low during rush hour, higher at other times)

This is a lot of factors to weigh against each other! It's easy to get lost in a sea of tradeoffs and what-ifs, and still not find any way to concretely define your goal. So what's the solution?

## A Solution
One approach is to write simple equations that describe your outcomes in terms of other variables. This will be somewhat theoretical and you will have to make some assumptions, but the method is useful even if it isn’t perfect.

### Email Signups
Let’s make a reasonable assumption: the number of people a street team signs up in a day is roughly equal to the number of people they talk to in an hour, times the percentage of people who are interested in the mission enough to sign up, times the number of hours the street team is out working. In other words:

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;S (signups) = T (number talked to per hour) * I (percent interested) * H (hours)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;200 people per hour * 3% are interested * 6 hours = 36 people signed up

If we assume that 1) H will be set by the NPO's budget, and 2) our subway stations will have at least 200 people passing through per hour (an easy minimum to meet in NYC), then the only variable we can influence in this equation is I (interest), which we can do by targeting parts of the city where subway riders will be more interested in supporting women in tech.

From our equation we can also see that if we double the percentage of interested riders, we will double the number of signups! So “interest in the mission” is a factor we should definitely focus on.

### Donations
So let’s say we’ve gained lots of email subscribers. At some point, the NPO will send out emails asking for donations to their cause. Only a certain percentage of those emailed will donate. This percentage is often called a conversion rate, and a simple equation to describe it is:

>D (number of donations) = S (signups) * C (conversion rate)

For our task, C is mostly beyond our control (it will be up the the NPO to manage this.) So it seems that we can only increase the number of donations by increasing S, the number of email signups. We talked about that in the previous section.

That said, with the proper subway station recommendations we can probably affect the average **dollar amount** of each donation received.

How can we do this? Well, the reason we thought to consider income above is because income **might** be correlated with the amount that someone donates. For a serious project this is something that we would have to verify. But for now let's take the assumption as true and explore where it leads.

If we assume that on average people will donate an amount equaling, say, .2% of their annual income, then it becomes clear that, all else being equal, sending canvassers to talk to subway riders with high incomes will effect larger donations:

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;A (amount donated) = G (gross annual income) * .2% * D (number of donations)

So, given our assumptions, "income" is another factor that we should include in our analysis.

Combining our equations for Signups, Donations, and Amount we see that:

>A = .2% * G (gross income) * T (number talked to/hour) * I (percent interested) * H (hours) * C (conversion rate)

Now we can easily answer questions like “how good is a subway station where riders have high interest in the mission but low income?” For two stations with average rider income of $45k and $80k, and with 6% and 3% of riders interested in the mission, respectively, our donation income is:

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;A (amount) = .2% * $45000 * 200 * 6% * 6 hours * 1% conversion rate = $86.20 (and 72 signups)  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;A (amount) = .2% * $80000 * 200 * 3% * 6 hours * 1% conversion rate = $57.60 (and 36 signups)

Despite the first group having much lower average income, we get more donation money and more email signups from that group because of their high level of interest in the NPO’s cause. In fact, it’s obvious from our equation that if Group A has double the interest of Group B, we will get more signups and donations from them unless their income is fully **half** that of Group B.

## Final Analysis - The 'Best' Stations
So here's what our final analysis might look like.


1. We know that we need a certain minimum of foot traffic in order to fully utilize our canvassers. So we start by ruling out times of day when foot traffic falls below that threshold. Above that threshold we assume that our canvassers will have plenty of people to talk to.

2. We come up with reasonable estimates for "percent interested in the NGO's mission" based on whatever data we can find that bears on that issue. If the mission is "increasing representation of women in tech," then perhaps we estimate interest based on subway proximity to tech hubs in the city.

3. We merge in our income data for various parts of the city.

4. With the subway stations and hours of the day that we have left after step 1, and the data from steps 2 and 3, we compute our expected donation income for each station.

5. Sort our stations by expected donation income. Someone with knowledge of New York City should look over the results and see if they make sense. If they do, then we're done!

We've successfully taken lots of useful variables - time, traffic, income, and mission interest - and created an algorithm that combines them in a meaningful way.
