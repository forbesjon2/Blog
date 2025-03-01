---
layout: single
title: "binomialOptions2018"
date: 2022-12-20 03:05:57 -0500
author: Jonathan Forbes
categories: 
  - Project Overview
permalink: "/binomialOptions2018"
---

## Project motivation

The idea for this project can be sourced from a 20 second clip in the movie [The Big Short](https://en.wikipedia.org/wiki/The_Big_Short_(film)). Which, for legal reasons I can't embed here so here is a link:

[https://youtu.be/Qo1OSqBQYmk?t=218](https://youtu.be/Qo1OSqBQYmk?t=218)

The clip explains a very simplified version of the investment strategy for a firm known in the movie as *Brownfield Fund* known as *Cornwall Capital* in real life. Betting on unlikely events using cheap options. The list of stocks came from a zacks.com screen. It had broad requirements aimed at any stock with a liquid enough option chain.

As for the strategy, I was hoping to find out what events the market thinks will never happen based off of the abnormal option pricing. Abnormal in this case just means that its expecting a move that doesn't match its past performance since volatility data is what the algorithm has to go off of.

[https://www.youtube.com/watch?v=PZrmOh2nZus](https://www.youtube.com/watch?v=PZrmOh2nZus)


## Technical notes

The code itself began as messy at best and later became somewhat understandable. It was written before I knew the best practices with respect to variable naming & program organization. The bulk of the data was initially sourced from Barchart's API before they required a subscription service. Then it was converted to TDAmeritrade's API in 2022. Stock volatility & dividend info is from yahoo finance.

## Example report (ran 11/27/22)

[Download Nov27-1.numbers](http://pointetelegraph.com/wp-content/uploads/2022/12/Nov27-1.numbers){: .wp-block-file__button}