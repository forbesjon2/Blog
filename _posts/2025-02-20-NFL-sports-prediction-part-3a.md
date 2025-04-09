---
layout: single
classes: wide
author_profile: false
sidebar:
  nav: NFL-part-2
title:  "NFL Sports Prediction Part 3a: Custom loss function"
date:   2025-02-17 22:20:41 -0500
categories:
  - Machine Learning
permalink: "/NFL-part-3a"
---
<style>
.scrollable-code {
  max-height: 300px; /* Set the desired max height */
  overflow-y: auto;  /* Enable vertical scrolling */
  overflow-x: hidden; /* Disable horizontal scrolling (optional) */
  background-color: #f8f8f8; /* Matches the code block's background */
  padding: 10px;     /* Optional: Adjusts the padding */
  border: 1px solid #ccc; /* Optional: Adds a border for aesthetics */
}
</style>

# Overview
Currently, we optimize


We explore creating a custom loss function and evaluation metric. The idea is to be able to include the odds data while preventing the model from predicting based on the odds alone. In theory, this saves us from having to enforce a limit based on the market odds of what bets should be placed or not.

## Inspiration
https://www.sciencedirect.com/science/article/pii/S266682702400015X
: Says “Hubáček and colleagues (2019) designed a loss function to penalise correlation with the bookmaker’s odds. One of the key findings of their work was that training models under this loss resulted in greater profits than optimising for accuracy. Further, the authors noted that a highly accurate predictive model is useless as long as it coincides with the bookmaker’s model.”

<br />
For this we will use Pea






# Composition
When creating a custom loss/objective function for pytorch, it's important to keep it differentiable. This then allows pytorch to compute gradients when `backward()` is called.

**Our loss function is composed of two parts:**
1. **[Pearson correlation coefficient](https://www.sciencedirect.com/topics/social-sciences/pearson-correlation-coefficient
)**:   This is used to penalize correlation with the bookmakers odds. It returns a number between 0 and 1. Where 0 shows no correlation. We multiply it by a hyperparameter `pearson_multiplier` which is defined externally and left unchanged within each trial.

2. **Kelly criterion**: Here we use the predicted odds (from the model) to calculate the correct position sizing for each bet. We penalize each correct bet by applying the pearson correlation coefficient and get the result. 

After we get the combined bets, we apply a cumulative sum and negate the result for Adam.

**Note** Write something here about the cumulative sum
  Original Array: [  1  -2   3  -4   5]
  Cumulative Sum (np.cumsum): [ 1 -1  2 -2  3]
  Total Sum (np.sum): 3
{: .notice}




# Results
Overall, this performed poorly. This is likely because it was trying to do too many things at once. For a dataset of this size we are restricted to a model that cannot be 

https://www.sciencedirect.com/topics/social-sciences/pearson-correlation-coefficient

**Note** Although there is a dedicated file `NFL_EDA-2.ipynb` for this step of the process, some of the EDA is performed in the previous `SlidingWindow-1.ipynb` file.
{: .notice}


