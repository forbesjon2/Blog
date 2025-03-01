---
layout: single
title: "Quick overview of the school social network app"
date: 2022-12-27 04:27:03 -0500
author: Jonathan Forbes
categories:
  - Project Overview
permalink: "/schoolSocialNetworkApp"
---

Development was halted when a similar app appeared. [Github](https://github.com/forbesjon2/MNO), [Demo](https://youtu.be/eT0aPIVplrM)

## Stack

**App:** react-native, redux, websockets (internally through JS, no NPM library), expo, jest (testing)  
**Web Server**: Node.js, express, ws, cassandra-driver (for scylla database), multer & aws-sdk (media upload)  
**Database:** scyllaDB

## Process

I followed the slower but more visually appealing process of designing first, developing the UI, web server, database, and putting everything together. Design was done in Adobe Illustrator using design references to examples on dribbble

<div class="image-gallery">
  <figure>
    <img src="{{site.baseurl}}/assets/2022/12/dptwo-543x1024.png" alt="Design preview 1">
  </figure>
  
  <figure>
    <img src="{{site.baseurl}}/assets/2022/12/threek-535x1024.png" alt="Design preview 2">
  </figure>
</div>

<div class="full-width-image">
  <figure>
    <img src="{{site.baseurl}}/assets/2022/12/twok-1024x490.png" alt="Design preview 3" width="655" height="313">
  </figure>
</div>