###Mixup and CutMix

1. Mixup : it mix up two different images and labels making desision boundary more vauge and doing generalization
2. CutMix : it cut off one part of image, and part of another one is attached to it

* both are used in Batch unit, which is not allowed to used in transform sequence that is adpated to each imageset.
* it is a quite strong technique to avoid overconfience of model and imporve generalization. especially,mixup contributes to overconfidence and cutmix helps to extract diverse features!
