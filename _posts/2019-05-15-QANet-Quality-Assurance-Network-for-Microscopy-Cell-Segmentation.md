---
#title: A Different Title
tags: [deep learning, bio-image-analysis]
category: papers
excerpt: 
---
- Some notes from reading
  + "QANet - Quality Assurance Network for Microscopy Cell Segmentation"
  + - [https://arxiv.org/abs/1904.08503](https://arxiv.org/abs/1904.08503)
- QANet is "a method for scoring the accuracy of any instance segmentation method, at the single image level, without the need for ground truth annotations or manual inspection of the target data."
- "The QANet does not in itself produce the segmentation of the image, but rather estimates the quality of a given segmentation"
- Based on RibCage, an LSTM network that the authors published
  previously
- The goal is to have a network that can estimate the quality of a
  proposed segmentation, without any other input than the image and
  the proposed segmentation.
- Training the network
  + Two sets of segmentations on the same image are required for the training of the network
  + In this work they take images and ground-truth from the Cell
    Segmentation benchmark - using the simluated dataset Fluo-N2DH-SIM+
  + They create the second segmentation by elastically deforming the
    ground truth
  + The network output is S_hat(I,\Gamma'), i.e., a function of the
    image and it's proposed segmentation - and of all the weights \theta
  + The loss function is the L2 difference between S_hat(I,\Gamma')
    and S(\Gamma,\Gamma'), i.e. between the the network output and
    the SEG score for the ground truth segmentation \Gamma and \Gamma'
  + The SEG score S, is the intersection over union IoU of the two segmentations
- Network prediction
  + The input now is only one proposed segmentation \Gamma'u and the
    image it is segmenting I
  + The magic happens in the RibCage network that has learned the
    connection between an image with segmentation and its SEG score
- Testing, after training on a specific image data set (SIM+) and
  ground truth segmentation of it
  + They generated "proposed" segmentations by running segmentation
    algorithms on different image data sets than used for training
    (GOWT1 and HeLa from the Cell Segmentation benchmark) then
    compared the predicted SEG score with the SEG score based on the
    ground truth segmentation of those images
  + The ground truth is the one supplied with the dataset and has been
    curated by humans, probably
  + They compared the performance of two alternative networks,
    Siamese and Classification, and found (unsurprisingly) RibCage to be superior when
    training on all three datasets and predicting the SEG score for
    the three different segmentation algorithms
  + The three segmentation algorithms were: BGFU-IL, CVUT-CZ, and
    KTH-SE, i.e., two DL and one classic method
- Winner
  + RibCage Network trained on 3-class segmentation (foreground,
    background, and cell contour) - when AUC is used as score (see
    comment below)
- Fine print
  + Still need ground truth segmentation results on image data similar
    to the one you want to predict on
  + Though the RibCage outperforms the other networks in terms of AUC
    (area under curve), in a plot of HitRate against IoU Prediction
    Tolerance, they all performed nearly identical at a tolerance over 50%,
    which is also the only thing that makes sense - actually, RibCage
    is sligthly worse than the others above 40%.
  + In some sense, using AUC looks a bit like a case of p-hacking (see
    comment above)
