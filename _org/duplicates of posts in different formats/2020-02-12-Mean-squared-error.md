---
title:  "Mean Squared Error: Love It or Leave It?"
tags:  [image-processing, signal-quality]
category: papers
excerpt: '"...an optimized system is only as good as the optimization criterion used to design it." Some MSE bashing, some SSIM praising.' 
mathjax: true
--- 

- Notes from reading the 2009 paper:
  + "Mean Squared Error: Love It or Leave It?"
  + by Zhou Wang and Alan C. Bovik
  + [https://ieeexplore.ieee.org/document/4775883](https://ieeexplore.ieee.org/document/4775883)
- This is not a scientific report, there are no new results, it is more of an overview or opinion piece. 
- The area of interest is quantification of signal-, mainly image-, fidelity, i.e., the construction of quantitative measures for *perceptual signal quality* that agree with what we know about the human perceptual system and with how humans, subjectively, score signal quality.
- Points out some of the many issues with using the mean-squared-error (MSE) as a measure for *signal-fidelity*. But also explains why the MSE is so widespread and therefore self-perpetuating.
  + Peak-signal-to-noise-ratio (PSNR) normally doesn't contain any new information compared to MSE
- In spite of the title, only the first part of the paper is about the MSE, then it covers other measures and their application beyond images (video, audio)
- This review is from before deep learning took off in 2012, it would be interesting to know how much has changed in the research area of signal fidelity since then. It is unlikely that it has survived unscathed.

## Mean Squared Error (MSE)
- Pros
  + Simple (to understand and calculate)
  + Mathematically well-behaved (differentiable, symmetric, convex)
  + Additive, when noise sources are independent
  + When using the L2 norm (not the L1 or other norms)
    * It is natural way to define the energy of the error signal
    * The energy is preserved under linear transforms, e.g. Fourier; meaning that the energy is the same in the signal and transform domains
- Cons
  + Fundamentally, the MSE is not sensitive to many of the aspects of images that are fundamental to our interpretation of them. 
  + The MSE is
    1. Independent of spatio-temporal structures in the signal
    2. Independent of relationship between signal and error
    3. Independent of sign of error
    4. Treating all data (samples) as equally important
	
## The Structural Similarity (SSIM) index
- Recasting the problem of signal-fidelity measurements in the language of /information communication/, the desiderata for a measure are that it makes good use of any information we many have about the *transmitter*, *channel*, and *receiver*.
  + Work along these lines go back to the 1970s
  + Transmitter: the clean/original/pristine image
  + Receiver: The human visual system
  + Natural images occupies a tiny portion in the space of all possible images
    * This is /prior knowledge/
    * Hypothesized that the human visual system is highly adapted to deal with natural images (makes sense)
- The structural similarity (*SSIM*) measure was introduced in 2002, by the same authors.
  + Main motivation: there is much structure in natural images, i.e., strong correlations between neighboring pixels and MSE doesn't capture this
  + Philosophy: the human visual system is a an ideal information extractor, identifying objects/structures in the visual scene
  + Seek to create a measure that is sensitive to structural changes (noise, blur, lossy compression) but insensitive to non-structural changes (brightness, gamma, spatial shift)
- The SSIM index is calculated from two images, A and B, by comparing the pixel intensities in small patches at the same location. It is the product of three terms
  1. The luminance (brightness), I(A,B) = 2\mu_{A} \mu_{B} / (\mu_{A}^{2} + \mu_{B}^{2})
  2. The contrast, c(A,B) = 2 \sigma_{A} \sigma_{B} / (\sigma_{A}^{2} + \sigma_{B}^{2})
  3. The structure, s(A,B) = \sigma_{AB} / (\sigma_{A} \sigma_{B})
- Here, \mu is the mean of the patch, \sigma the standard deviation, and \sigma_{AB} the crosscorrelation
- It is clear, that the SSIM index takes on values from -1 to +1 and is symmetric in the arguments. Also, the nominator in the  contrast term cancels with the denominator in the structure term, so really there are just two terms in the SSIM
  + Usually, small stabilising terms (scalars) are added to the nominator and denominator in each of the three terms, to avoid dividing with zero when the mean or variance vanishes; so there are three additional parameters to adjust, in addition to the patch size. Often, these scalars can be set to zero.
- The SSIM index is calculated for each pixel in an image, based on a patch of variable size, then averaged to give a single number for the entire image. The average can be a simple arithmetic mean, or some space-varying weighted mean.
- The SSIM index is quite robust against (returns high values for) changes in luminance shifting, contrast stretching, and additive noise on highly patterned backgrounds. This is consistent with  humans visual perception
- However, it is highly sensitive to (returns low values for) shifts, rotations, scaling (zooming in/out), and additive noise in homogeneous regions. This is not consistent with human visual perception (except maybe for the noise part) 
- To address the shortcomings of the SSIM measure, and inspired by how it fails, a new measure was developed:

## The Complex Wavelet SSIM (CW-SSIM) index
- The CW-SSIM was developed in 2005 and sounds like a very good idea. But, for some reason, hardly anyone is citing that work.
- The CW-SSIM index is the product of two terms that capture
  1. The *magnitude* of wavelet coefficients, m
  2. The consistency of *phase* changes
- Probably worth visting the 2005 paper for a careful description, since this review doesn't give details on how the transform is done and what is subbands and coefficients are combined in the measure.

## Restoration and classification
- This is an area that has been completely dominated by deep learning since 2012, so any triumphs of SSIM in this arena will have been short-lived. Still, it is instructive to see how far you can go with very little, and just plain funny to see what happen when you try to apply one idea (SSIM) to just about everything you can think of.
- For image restoration, when the problem is simple, i.e. when the distorted signal is simply a linearly blurred version with additive noise, it is well known that the Wiener filter is optimal --- it minimises the MSE (the expectation of the squared error). But what happens if we minimise another measure, say, the SSIM?
  + Not surprisingly, SSIM-optimal linear filters, that minimise the stat-SSIM (very similar to SSIM), perform better than MSE based filters, in terms of visual perceptual fidelity.
- For classification, the toy model is hand written digits, and CW-SSIM returns an impressive 97.7% accuracy. 
  + However, this is not MNIST data, these are test images that are generated from ground-truth images, via shifting, scaling, rotation, and blurring.
  + The classification is then a matter of calculating the CW-SSIM between a test image and each of the ten ground-truth images, and then classify the test image as the digit corresponding to the pairing with the highest score.
  + The important difference from MNIST, is that here there is only one ground-truth set of neatly typed hand-written digits and all the test images are generated from this set. In MNIST, each person writing has their own ground-truth --- some people use serifs on their numbers, others don't etc.
  
## Resources
- Further reading
  + Wikipedia on SSIM
    * [https://en.wikipedia.org/wiki/Structural_similarity](https://en.wikipedia.org/wiki/Structural_similarity)
  + SSIM at a single scale
    * 2002 (short read): [A universal image quality index](https://ieeexplore.ieee.org/document/995823)
    * 2004 (highly cited paper): [Image quality assessment: from error visibility to structural similarity](https://ieeexplore.ieee.org/document/1284395)
  + SSIM over multiple scales
    * 2003: [Multiscale structural similarity for image quality assessment](https://ieeexplore.ieee.org/document/1292216)
  + SSIM for wavelets
    * 2005: [Translation insensitive image similarity in complex wavelet domain](2005: https://ieeexplore.ieee.org/document/1415469)
  + A comparison of PSNR and SSIM
    * 2010: [Image Quality Metrics: PSNR vs. SSIM](https://ieeexplore.ieee.org/document/5596999)
