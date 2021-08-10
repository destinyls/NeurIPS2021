# Response to reviwer B8kL 

标签： NeurIPS2021_Rebuttal

---
**Q1**: The technical part of this paper has merits. However, some pieces of ideas in their design are not totally new. Using multi-scale feature maps to capture objects in different scales is a popular strategy in object detection and segmentation works. Sampling key points in feature maps was also investigated in 2D object detection methods (e.g., SaccadeNet: A Fast and Accurate Object Detector, CVPR2020) and 3D object detection methods (e.g., Infofocus: 3d object detection for autonomous driving with dynamic information modeling, ECCV2020).

**A1**: Feature pyramid network (FPN) has been an effective framework to extract multi-scale features in object detection. And there appears a lot of modified FPN structures, such as NAS-FPN(CVPR2019), BiFPN(CVPR2020), AugFPN(CVPR2020), CE-FPN(CVPR2021), etc. Most of these methods are committed to achieving high detection accuracy without regard to efficiency, which inevitably brings about longer latency and NMS post-process. There are almost no FPN frameworks emphasizing lightweight design. For this reason, we proposed a novel Lite-FPN module that implements feature alignment and fusion based on keypoint sampling and thus avoids the additional detection heads and the huge fusion operation of feature maps. Experiments based on SMOKE and CenterNet on KITTI datasets show that our proposed FPN architecture can significantly improve accuracy and meanwhile reduce inference time. It is possible that the lightweight design of FPN will be a practical research focus, through which we can achieve comparatively good accuracy and speed trade-off.

---

**Q2**: Did you quantitatively analyze whether localization precision is the major failure case compared to classification? A straightforward way would be (1) using the groundtruth class for each prediction and keep the location prediction and (2) using the groundtruth location and keep the predicted class. Then, see which one improves the performance more.

**A2**: Firstly, we would like to thank your constructive suggestions. Our proposed attention loss is effective in alleviating the misalignment between localization precision and classification score. However, the classification score in CenterNet-like models represents the probability of keypoint in each pixel and related class. Hence, we fail to conduct the experiments mentioned above for the sake of the unavailable confidence score in the ground truth.

---

**Q3**: KITTI is a relatively small dataset. Experiments on larger dataset like nuScenes would be more beneficial.

**A3**: Our proposed Lite-FPN and attention loss are generic modules that can be integrated into most CenterNet-like models.
We choose SMOKE and CenterNet as two baselines, which are both primarily demonstrated on KITTI. To equally compare our approach with two baselines, KITTI is adopted as the primary benchmark. Also, most popular methods like D4LCN(2020) and Ground-aware(2021) conduct experiments mainly on KITTI. Therefore, KITTI is adopted heavily in our experiments. It is absolutely necessary to evaluate our approach on the newer and bigger dataset such as nuScenes and Waymo. Unfortunately, the related results of baselines have not been reported yet, which is not suitable for a fair comparison. However, we would test our algorithm on more datasets in our future work.

---

**Q4**: Missing the comparison with the SOTA methods in general.

**A4**: We update more SOTA methods in the following table. 
As shown in Table 8, although D4LCN and Ground-Aware possess relatively higher precision, the latency of D4LCN is too long to satisfy runtime requirements. Ground-Aware may possess a more practical frame rate, whereas it’s hard to migrate it to another existing method. By contrast, our proposed Lite-FPN and attention loss are generic modules that can be integrated into most CenterNet-like models; thus, we can achieve higher accuracy with newly released baselines. Furthermore, our improved SMOKE with ResNet-34 backbone reaches 71.32 FPS on 2080Ti and achieves a comparatively good speed-accuracy trade-off.

| Methods | Backbones | GPU | FPS | $E_{3D}$ | $M_{3D}$ | $H_{3D}$ | $E_{bv}$ | $M_{bv}$ | $H_{bv}$ |
|:---:|:---:|:---:|:---:|:---:| :---:|:---:|:---:|:---:|:---:|
| FQNet | VGG-16 | 1080Ti | 2.00 | 2.77 | 1.51 | 1.01 | 5.40 | 3.23 | 2.46 |
| MonoGRNet | VGG-16 | Tesla P40| 16.67 | 9.61 | 5.74 | 4.25 | 18.19 | 11.17 | 8.73 |
| MonoPSR | ResNet-101 | Titan X | 8.34 | 10.76 | 7.25 | 5.85 | 18.33 | 12.58 | 9.91 |
| MonoDIS | ResNet-34 | Tesla V100 | 10.00 | 10.37 | 7.94 | 6.40 | 17.23 | 13.19 | 11.12 |
| UR3D | ResNet-34 | Titan X | 8.33 | 15.58 | 8.61 | 6.00 | 21.85 | 12.51 | 9.20 |
| M3D-RPN | DenseNet-121 | 1080Ti | 6.25 | 14.76 | 9.71 | 7.42 | 21.02 | 13.67 | 10.23 |
| MonoPair | DLA-34 | 1080Ti | 17.54 | 13.04 | 9.99 | 8.65 | 19.28 | 14.83 | 12.89 |
| RTM3D | DLA-34 | 1080Ti | 18.18 | 14.41 | 10.34 | 8.77 | 19.17 | 14.20 | 11.99 |
| Ground-Aware | ResNet-101 | 1080Ti | 20.00 | 21.65 | 13.25 | 9.91 | 29.81 | 17.98 | 13.08 |
| D4LCN | ResNet-50 | Tesla V100 | 5.00 | 16.65 | 11.72 | 9.51 | 22.51 | 16.02 | 12.55 |
| SMOKE| DLA-34 | 2080Ti | 40.32 | 14.03 | 9.76 | 7.84 | 20.83 | 14.49 | 12.75|
|Ours-SMOKE| ResNet-34| 2080Ti | 71.32 | 15.32 | 10.64 | 8.59 | 26.67 | 17.58 | 14.51 |
|Ours-SMOKE| DLA-34 | 2080Ti |42.37 | 15.73 | 10.97 | 9.02 | 24.65 | 16.38 | 14.52 |

Table. 5: Results on the KITTI test 3D and BEV detection benchmark. Both metrics for car category are evaluated by AP40 at 0.7 IoU threshold.