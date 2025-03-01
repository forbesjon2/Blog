---
layout: single
title: "SpliceX project outline"
date: 2022-12-28 03:05:16 -0500
author: Jonathan Forbes
categories:
  - Project Overview
permalink: "/SpliceX"
---

The motivation behind this project came from looking for the most efficient solution to a particular video editing task: voice splicing

## Overview

The audio or video files are transcribed and split into word bubbles. The word bubbles are colored based on transcription confidence. Clicking on the words plays the segment within the audio/video file.

http://pointetelegraph.com/wp-content/uploads/2022/12/SpliceX.mov


There's a search bar <time>0:34</time> that allows quicker searching of the word bubbles.

⌘ + Click on the word bubble <time>0:34</time> adds it to the word scroll list (bottom view).

The text or time for individual words in the transcription <time>0:46</time> can be modified to something else.

By default, users run audio playback but there also includes <time>1:13</time> video playback. This is somewhat buggy.

Each word bubble in the word scroll list can be selected and <time>0:46</time> the time adjusted.

Within the file list, the speaker column is currently unimplemented.

## Design notes

I decided not to go through the design phase for a project like this since it is more like a utility. This is unlike the last two larger projects: a school [social network app](https://youtu.be/eT0aPIVplrM) and the [worldcomm](https://youtu.be/ILDwSEABlCs) project. It usually includes a few weeks worth of making screens on illustrator or Figma. The objective is to get it completed faster than it took to make those.

## Technical notes

The swift project uses storyboards and was the first project that I decided to include code Linting midway through the project using [swift lint](https://github.com/realm/SwiftLint). It uses unit tests for certain functions. Initially I intended to use core data but decided to just import & export projects as JSON files.