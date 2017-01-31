---
layout: post
title: Regression - Finding a Function to Fit Your Data
---

Here's a simple dataset - two variables we'll call x and y.

*scatter plot, no line*

It looks like x and y are related! Let's say we want to use x to predict y. We can use regression to fit a function to the data. Here it is:

*scatter plot with line*

This data was easy to fit, because for every unit increase in x, y also increases some proportionate amount. The equation for the above line is:

y = 2x

What do we do when fitting the data isn't so easy, such as when our data looks like:

*quadratic scatter plot with line*

This function doesn't fit the data very well. The equation for this function is:

$$h(x) = af(x) + bg(x) \\rightarrow h'(x) = af'(x) + bg'(x)$$  
$$y = -.1x + 81.6$$

From the plot, it doesn't look like y varies as a function of x. Rather, it looks like y varies as a function of x^2. This means we want to look for a function of the form y = Bx^2 + Bx + B. Our regression program will take care of finding the best B values; we just need to give it an x variable **as well as** an x^2 variable to work with. [[maybe say more here about what this data actually is, i.e. health vs. weight]]

So I square my x values, and now I have data that looks like this. Note the new variable consisting of squared x's:

*table of data with x, x^2, and y*

I run regression to predict y in terms of these two other variables, and I get:

*scatter with quadratic line. Include equation*

That worked pretty well! If I had more variables these results would be harder to visualize, but my process would be the same: I would try to understand how my y variable varies as a function of my x variables. (Graphing each x, y scatter plot would be particularly useful.)

Let's try an example with three variables (two x and one y). Since we have three variables now, we'll have to visualize them in 3-D (if we had **more** variables, we'd have to give up on doing these kinds of scatter plots; they just wouldn't work.)

*scatter, no line*


