---
title:  "Estimation of motility parameters from trajectory data"
tags:  [cell-migration, data-analysis, time-series-analysis]
category: papers
excerpt: ''
mathjax: true
--- 


- Notes from reading the 2015 paper:
  + "Estimation of motility paramters from trajectory data"
  + by C.L. Vestergaard, J.N. Pedersen, K.I. Mortensen, and H. Flyvbjerg
  + [https://link.springer.com/article/10.1140/epjst/e2015-02452-5](https://link.springer.com/article/10.1140/epjst/e2015-02452-5)

## Short take
- **Input**: Time-series of positions of individual particles that undergo pure Brownian motion, i.e., diffusion without drift-, memory, or drive-terms. 
  + Cannot be anomalous either, so super- and sub-diffusion is ruled out.
  + The detected positions can have localisation errors, as long as these are independent from frame to frame (again no memory allowed), i.e. artefacts as seen in interlaced CCD readout is not covered here
  + The detected positions can have motion blur and this is modeled directly and straightforwardly as an effect of finite acquisition time for each frame
- **Output**: Good estimates of the diffusion coefficient and the amplitude of the tracking error
  + By "good" is meant that they are unbiased and, nearly, optimal in an information theoretical sense - they get close to the Cramer-Rao bound
  + Also, importantly, error-bars on the estimates are returned
  
## Not treated
- Any data that does not result from a purely diffusive process
- Tracking errors that are more complicated than simple additive white Gaussian noise on the positions
- Effects of crowding or obstacles - the particles have to behave like an indeal gas

## Items of note
- When they say "motility parameters" they really only mean the diffusion coefficient, at least that is the only motility parameter they derive any results for, others are only mentioned as unsolved
- They carefully treat and discuss the effect of **finite experiments**, i.e. experiments where the time-series for any one tracked particle can be quite short
- One source of potential bias resides in the decision on **when to stop an experiment** where multiple particles are observed in the same region of interest
  + Stop when the first particle leaves
  + Otherwise there is implicit bias: other particles may be slower or stuck or confined or moving less straight  or ...
  + Caveat: I believe that this does not apply if we positively know that we are observing diffusion, since we can then think of all observed particles as independent of each other - as if they had been recorded in separate experiments on other days. In this case, the experiment stops when we want it to, e.g. when all particles have diffused out and no more are coming in.
- The **localisation errors** can be from different sources and add up
  + Finite photon count gives rise to uncertainty in the determination of its position
  + Round-off errors due to the discreteness of pixels in images, even with infinitely many photons
  
## Resources
