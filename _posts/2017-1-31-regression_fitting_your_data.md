---
layout: post
title: Regression - Finding a Function to Fit Your Data
---

Here's a simple dataset - two variables we'll call \\( x \\) and \\( y \\).

![x,y scatter plot](/images/straight_scatter.png)

It looks like \\( x \\) and \\( y \\) are related! Let's say we want to use \\( x \\) to predict \\( y \\). We can use regression to fit a function to the data. Here it is:

![x,y scatter plot with regression line](/images/straight_line.png)

This data was easy to fit, because for every unit increase in \\( x \\), \\( y \\) also increases some proportionate amount. The equation for the above line is \\( y=2x \\), so we know that for every unit increase in \\( x \\), \\( y \\) increases by about 2.

What do we do when fitting the data isn't so easy, such as when our data looks like this?

![x,y quadratic scatter plot](/images/curve_line.png)

The equation for this function is \\( y=81.6-.1x \\), and it **doesn't** fit the data very well. Rather than \\( y \\) varying as a function of \\( x \\), it looks like \\( y \\) varies as a function of **\\( x^2 \\)**. This means we want to look for a function of the form \\( y=\beta\_0+\betax\_1+\beta\_2x^2 \\). Our regression program will take care of finding the best \\( \beta \\) values; we just need to give it an \\( x \\) variable **as well as** an \\( x^2 \\) variable to work with.

So I square my \\( x \\) values, and now I have data that looks like this. Note the new variable consisting of squared \\( x \\)'s:

![table of data](/images/x_squared_data.png)

I run regression to predict \\( y \\) in terms of these two other variables, and I get:

![x,y scatter plot with quadratic regression line](/images/curve_curve.png)

That worked pretty well! The principle here is to try and understand how my \\( y \\) variable varies as a function of my \\( x \\) variables. Sometimes, having an understanding of the subject matter that you're dealing with can help. Let's look at one final, more difficult example where this will definitely be true.

In my final example we have three variables (two \\( x's \\) and one \\( y \\)). Since we have three variables, we'll have to visualize them in 3-D.

![x1, x2, y scatter plot](/images/scatter_3d.png)

This data is made up, but imagine it comes from a psychology experiment where participants complete tasks of varying difficulty after being kept awake for varying numbers of hours. The experimenter measures how participant response time (in milliseconds) on the tasks changes as a function of task difficulty and sleep deprivation.

Let's again try to fit a function to this data without doing any kind of transformation beforehand. Here's what we get (
note that we have a **plane** now, instead of a line, because we're dealing with 3 dimensions.)

![x1, x2, y scatter plot with regression plane](/images/straight_plane.png)

Our function for this plane is \\( y=22.6x\_1+14x\_2+316.1 \\)  
Our function doesn't fit the data very well. The bottom right corner of the plane is too high, whereas the top right corner is too low to fit the data correctly.
