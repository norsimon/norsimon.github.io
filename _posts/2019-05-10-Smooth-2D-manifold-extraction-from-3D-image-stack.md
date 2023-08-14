---
#title: A Different Title
tags: [image-processing, microscopy]
category: papers
excerpt: "An alternative to maximum intensity projection or wavelet based z-stacking/focus-stacking/extended depth of field"
---
- [https://www.nature.com/articles/ncomms15554](https://www.nature.com/articles/ncomms15554)
- An alternative to maximum intensity projection or wavelet based z-stacking/focus-stacking/extended depth of field
- Fiji plugin available, no parameters to set
- Idea: "ﬁt a ‘smooth’, parameter-free, 2D manifold onto the foreground signal while ‘ignoring’ the background"
- Works on 2.5D images, be careful with real 3D imaging
  + "in the case of a full 3D content, a single 2D extraction cannot be satisfactory"
- "Selecting a reference channel to compute a unique index map for all
  channels might be regarded at ﬁrst as a parameter. However, it is
  not, it should be a rational choice related to the biological
  question being tested with a given data set"
- How it works
  + For each x,y position, the focal score is calculated as a function
    of z: F(z)
  + The Fourier transform (PSD) is then calculated for each F(z) - the
    idea being that x,y positions corresponding to forground objects
    have low-frequency power and not just (background) noise
  + A 3-class k-means is performed and each x,y position assigned a label: foreground, background, uncertain
  + The resulting 2D index map of the labels, build of these three classes
    is smoothed
  + A 2D image is formed, taking the intensity value for each x,y
    corresponding to the z position in the smoothed index map
- I did not try to understand the details of how they smooth and why
  there are no free parameter. This is covered in their supplementary information
