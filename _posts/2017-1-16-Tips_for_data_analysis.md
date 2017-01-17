---
layout: post
title: Tips for Data Analysis: An Example Using MTA Turnstile Data
---

Imagine a non-profit organization called Women Tech Women Yes (WTWY) wants you to consult on a data science project. The organization is having a gala in the summer to promote their mission of increasing representation of woman in tech. To get the word out, WTWY plans to send street canvassers to various subway stations around New York City, where they will solicit email addresses and drum up interest in the WTWY summer gala. WTWY has asked you to help optimize the location of their street teams. Which subway stations should they send teams to?

You've decided to use data from the MTA turnstiles to assess foot traffic at each subway station. The rest of your task is quite open-ended in terms of how you choose the "best" locations, but you figure that the turnstile data is a good place to start.

The turnstile data will tell you (after some data cleaning) how many people entered and exited each subway station in New York every hour of every day of the year. Given this information, there are many factors you might consider when choosing the best location for WTWY's street teams:
- stations with high traffic
- time of day at which traffic volume is high
- weekdays versus weekends
- number of tourists vs. NYC residents
- ability/willingness of people to donate to WTWY's cause
- interest in WTWY's mission



## The Problem
The hardest part of an open-ended data science project is often defining concretely what your goal is. For example, if you work for a non-profit organization trying to 1) gather email subscribers who support your mission, and 2) collect donations,  which of those things is more important? And are you willing to sacrifice one for the other? The more variables you have to consider, the harder it is to determine their relative importance.

For my first project at Metis Data Science Bootcamp, I worked with a team using passenger entrance/exit data from the New York subway system to determine the optimal placement of street canvassers (i.e. those wonderful people asking you to sign up for things.) Our very open-ended goal was to support the work of a non-profit that was looking to collect email addresses and, subsequently, donations. What follows are some thoughts on how to choose a concrete goal and implement it.

For this project we needed to choose the "best" subway locations for canvassers to solicit email addresses... but it was up to us to determine what "best" might mean. Here are just some of the factors we thought to take into account:
- volume of foot traffic into/out of the subway stations
- income of passengers at each station
- proximity to universities
- proximity to tech hubs in NYC (this was relevant to the mission of the NPO, which was to increase representation of women in tech)
- passenger responsiveness to solicitation (probably low during rush hour, higher at other times)

Ultimately what we need is a list of the top 15 or so stations to send canvassers to. This means we need to rank each station. But how do we determine a rank from all of the variables that we want to consider? If a station is in the 90th percentile for traffic volume, but the 50th percentile for income, should it be ranked higher or lower than a station in the 50th percentile for traffic and the 90th percentile for income? And do we have to compute these rankings for different times of day, or different days of the week? It's easy to get lost in a sea of tradeoffs and special cases, and still not find a way to concretely determine what your goal is.

## A Solution
One approach is to write simple equations that describe what variables (in theory) affect your outcomes. For example, the number of people a street team signs up in a day ('S') is perhaps equal to the number of people they can engage in conversation in an hour ('E'), times the percentage of those people who are interested in the organization ('I'), times the number of hours the street team is out working ('H'). In other words:
S = E * I * H
200 people per hour * 3% are interested * 6 hours = 36 people signed up

Now we can probably assume that H will be dictated by WTWY's budget and what they are able to pay for, and we can assume that E is a matter of canvasser talent and therefore mostly beyond our control. Given that H and C are set, the only variable in this equation that we can affect is I - the level of interest in WTWY's mission. We can see that increasing I should directly increase the number of signups we get. In the above example, 36 people sign up. But if our street team were located near a tech hub, where say 5% of the passers-by were interested in WTWY's mission, we might see 60 people sign up - almost double the previous amount. This solidifies the idea that targeting subway stations based on tech interest is of high value to us.

Another detail that's relevant to the above equation is the station traffic (entrances plus exits) per hour. Our calculations only hold if station traffic is at least 200 people per hour. Fortunately, in New York City virtually every station on average satisfies this requirement (though we may need to break the data down by time of day). So, while it was common for various Metis teams to focus on 'high station traffic' in their analyses (my team included), there probably is a threshold effect, where having greater traffic doesn't increase your ability to get signups, because you can't talk to most of those passers-by anyway. As long as the stations and hours that we recommend meet this minimum traffic threshold (say, 200 people per hour), the equation above holds true.

## Donations
Donations come after signups. Some percentage of the people who sign up for WTWY emails will give a donation (this percentage is often called a conversion rate):
D (number of donations) = S (signups) * C (conversion rate)

The conversion rate will depend on how WTWY handles their email list, so from our perspective let's assume that C is set. We can only increase the number of donations by increasing the number of email signups. However, can we affect the **size** of the donations?

We made the assumption above that a person's income is correlated with the size of their donation (this may not be true, but let's explore where it takes us). If we assume that a person might give a donation equaling .1% of their annual income, then it becomes clear that, all else being equal, sending street teams to talk to subway riders with high incomes will effect larger donations:

A (amount donated) = G (gross annual income) * .1% * D (number of donations)

200 * 6% * 6 hours = S = 72
S * 1% = D = .72
D * 60000 * .1% = A = $43.2

200 * 3% * 6 hours = S = 36
S * 1% = D = .36
D * 80000 * .1% = A = $28.8

A = .001GEHIC
