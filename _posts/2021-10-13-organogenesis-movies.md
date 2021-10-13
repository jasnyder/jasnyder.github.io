---
title: 'Cell polarity model'
date: 2021-10-13
permalink: /posts/2021/10/organogenesis-movies/
tags:
  - research
  - organogenesis
  - visualization
---
I am working on a model of organ growth with the aim of simulating the development of branched structures. As a basis I am taking the model used in this paper:
* Nielsen BF, Nissen SB, Sneppen K, Mathiesen J, Trusina A. Model to Link Cell Shape and Polarity with Organogenesis. iScience. 2020 Feb 21;23(2):100830. doi: [10.1016/j.isci.2020.100830](https://doi.org/10.1016/j.isci.2020.100830). Epub 2020 Jan 11.

You can see some videos of the dynamics by clicking the links below. Animations are directly embedded in HTML
* [sphere initial condition](/files/animations/sphere-test.html)
* [tube initial condition](/files/animations/tube-grid-test.html)
* [tube initial condition with controlled rate of cell division](/files/animations/tube-grid-slowly.html)

The code I am using was written by Julius Kirkegaard, and is avialable [here](https://github.com/jasnyder/polar). I am using [Plotly](https://plotly.com/) to generate the animations.