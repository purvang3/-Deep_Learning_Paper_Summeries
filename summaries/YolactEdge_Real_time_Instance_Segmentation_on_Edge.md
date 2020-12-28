[YolactEdge: Real-time Instance Segmentation on the Edge
(Jetson AGX Xavier: 30 FPS, RTX 2080 Ti: 170 FPS)](https://arxiv.org/abs/2012.12259)

Paper proposes instance segmentation approach that runs with 3-5x faster speed over existing real-time methods while producing
competitive mask and box detection accuracy on edge devices at real-time speeds using TensorRT
optimization and  a novel feature warping module to exploit temporal
redundancy.

#### Introduction
Instance segmentation is a challenging problem that requires the correct detection and segmentation of each object
instance in an image. this paper uses SOT YOLACT and make two fundamental changes.
- using NVIDIA’s TensorRT inference engine to quantize the network
parameters.
- reducing temporal redundancy of feature computation in nearby frames are highly correlated.

#### RELATED WORK
All existing real-time instance segmentation approaches are image-based and require bulky GPUs.
Feature propagation in videos has been used to improve speed and accuracy for video classification and video object
detection using off-the-shelf optical flow networks, which adds latency, to estimate pixel-level object
motion and warp feature maps from frame to frame. On the other hand, paper presents method that estimates object motion 
and performs feature warping directly at the feature level instead of pixel level.
There has been many approaches to build efficient architectures
-  depth-wise convolutions and inverted residuals (MobileNetv2)
-  neural architecture search (MobileNetv3, NAS-FPN, and EfficientNet)
- TensorRT a deep learning inference optimizer, to quantize and speed up

In contrast, YolactEdge retains large expressive backbones, and exploits temporal 
redundancy in video together with a TensorRT optimization for fast and accurate instance segmentation.

#### APPROACH
TensorRT: Paper adopts method  that converts each model component(feature backbone, FPN, ProtoNet, head) to TensorRT 
independently and explore the optimal mix between INT8 and FP16 weights that maximizes FPS while preserving accuracy.
It has been observed that conversion of the Prediction Head to INT8 always results in a large loss of instance segmentation
accuracy and Converting every component to INT8 except for the Prediction Head and
FPN achieves the highest FPS with little mAP degradation.

Since INT8 precision requires calibration, TensorRT collects histograms of activations for each layer, generates several
quantized distributions with different thresholds, and compares each quantized distribution to the reference distribution
using KL Divergence. This step ensures that the model loses as little performance as possible when converted to
INT8 precision.

Exploiting Temporal Redundancy in Video: In video, exploit can be made to reduce temporal redundancy to make
YolactEdge even faster. YoloactEdge perform two task in parallel.
- generating a set of prototype masks
- predicting per-instance mask coefficients. Then, the final masks are assembled through linearly combining the 
prototypes with the mask coefficients.

Partial Feature Transform is performed for a non keyframe backbone features and skipping inter mediate resolutions, 
instead of using optical flow for all features. 

Efficient Motion Estimation: compute flow between keyframe and non-keyframe
#### Loss function used for training
classification loss, box regression loss, mask loss, and auxiliary semantic segmentation loss
and for flow estimation network pretraining- endpoint error.


