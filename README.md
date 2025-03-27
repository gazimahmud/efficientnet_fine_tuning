# efficientnet_fine_tuning

## EfficientNet

EfficientNet, first introduced in  [Tan and Le, 2019](https://arxiv.org/abs/1905.11946)  is among the most efficient models (i.e. requiring least FLOPS for inference) that reaches State-of-the-Art accuracy on both imagenet and common image classification transfer learning tasks.

The smallest base model is similar to  [MnasNet](https://arxiv.org/abs/1807.11626), which reached near-SOTA with a significantly smaller model. By introducing a heuristic way to scale the model, EfficientNet provides a family of models (B0 to B7) that represents a good combination of efficiency and accuracy on a variety of scales. Such a scaling heuristics (compound-scaling, details see  [Tan and Le, 2019](https://arxiv.org/abs/1905.11946)) allows the efficiency-oriented base model (B0) to surpass models at every scale, while avoiding extensive grid-search of hyperparameters.

A summary of the latest updates on the model is available at  [here](https://github.com/tensorflow/tpu/tree/master/models/official/efficientnet), where various augmentation schemes and semi-supervised learning approaches are applied to further improve the imagenet performance of the models. These extensions of the model can be used by updating weights without changing model architecture.

----------

## B0 to B7 variants of EfficientNet

_(This section provides some details on "compound scaling", and can be skipped if you're only interested in using the models)_

Based on the  [original paper](https://arxiv.org/abs/1905.11946)  people may have the impression that EfficientNet is a continuous family of models created by arbitrarily choosing scaling factor in as Eq.(3) of the paper. However, choice of resolution, depth and width are also restricted by many factors:

-   Resolution: Resolutions not divisible by 8, 16, etc. cause zero-padding near boundaries of some layers which wastes computational resources. This especially applies to smaller variants of the model, hence the input resolution for B0 and B1 are chosen as 224 and 240.

-   Depth and width: The building blocks of EfficientNet demands channel size to be multiples of 8.

-   Resource limit: Memory limitation may bottleneck resolution when depth and width can still increase. In such a situation, increasing depth and/or width but keep resolution can still improve performance.

As a result, the depth, width and resolution of each variant of the EfficientNet models are hand-picked and proven to produce good results, though they may be significantly off from the compound scaling formula. Therefore, the keras implementation (detailed below) only provide these 8 models, B0 to B7, instead of allowing arbitray choice of width / depth / resolution parameters.
