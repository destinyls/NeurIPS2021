# Response to reviwer 9nui 

标签： NeurIPS2021_Rebuttal

---
**Q1**: Although it is clearly reported in the method section that Light-FPN moves the top-K operations from post-processing to detection, the novelty of the proposed module is limited being more a different procedure than a new FPN architecture.

**A1**: Feature pyramid network (FPN) has been an effective framework to extract multi-scale features in object detection. And there appears a lot of modified FPN structures, such as NAS-FPN(CVPR2019), BiFPN(CVPR2020), AugFPN(CVPR2020), CE-FPN(CVPR2021), etc. Most of these methods are committed to achieving high detection accuracy without regard to efficiency, which inevitably brings about longer latency and NMS post-process. There are almost no FPN frameworks emphasizing lightweight design. For this reason, we proposed a novel Lite-FPN module that implements feature alignment and fusion based on keypoint sampling and thus avoids the additional detection heads and the huge fusion operation of feature maps. Experiments based on SMOKE and CenterNet on KITTI datasets show that our proposed FPN architecture can significantly improve accuracy and meanwhile reduce inference time. It is possible that the lightweight design of FPN will be a practical research focus, through which we can achieve comparatively good accuracy and speed trade-off.

---

**Q2**: The writing quality and the clarity in the explanations are largely below the acceptance threshold for the NeurIPS conference.

**A2**: Although some other reviewers consider the paper is well written, we will polish our writing to make the explanations clear.

---

**Q3**: In Table 5, some important recent baselines are missing in the comparison such as M.Ding et. al "Learning depth-guided convolutions for monocular 3d object detection” and, Liu et. Al “Ground-aware monocular 3d object detection for autonomous driving” which, on the same KITTI benchmark, perform better than the proposed method.

**A3**: As shown in Table 8, although D4LCN and Ground-Aware possess relatively higher precision, the latency of D4LCN is too long to satisfy runtime requirements. Ground-Aware may possess a more practical frame rate, whereas it’s hard to migrate it to another existing method. By contrast, our proposed Lite-FPN and attention loss are generic modules that can be integrated into most CenterNet-like models; thus, we can achieve higher accuracy with newly released baselines. Furthermore, our improved SMOKE with ResNet-34 backbone reaches 71.32 FPS on 2080Ti and achieves a comparatively good speed-accuracy trade-off.


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

---

**Q4**: Experiments are performed only on the KITTI dataset and only for the car class. The addition of detection benchmarks such as nuScenes and the extension to all the benchmark classes are needed for a clear and fair comparison with existing methods.

**A4**: Besides the car class, we add the results of pedestrian and cyclist classes in Table 1, to verify the generalization of our proposed solutions. As shown in Table1, our improved SMOKE achieves the performance improvements of 3.38%, 2.75% and 0.86% for car, pedestrian and cyclist respectively on moderate level over the baseline, which verifies the generalization of our proposed solutions on different categories.

| Category | Method | $E_{3D}$ | $M_{3D}$ | $H_{3D}$ | $E_{bv}$ | $M_{bv}$ | $H_{bv}$ |
|:---:|:---:|:---:|:---:|:---:| :---:|:---:|:---:|
| Car |  SMOKE  |  12.11 | 10.64 | 10.23 | 16.07 | 13.19 | 11.68 |
| Ped |  SMOKE  |  3.32 | 3.09 | 2.88 | 4.48 | 4.38 | 4.20 |
| Cyc |  SMOKE  |  0.30 | 0.27 | 0.27 | 0.41 | 0.38 | 0.37 |
| Car | Ours-SMOKE |  17.04 | 14.02 | 12.23 | 23.85 | 19.68 | 16.91 |
| Ped | Ours-SMOKE |  6.46 | 5.84 | 5.86 | 6.77 | 6.51 | 6.05 |
| Cyc | Ours-SMOKE |  1.14 | 1.13 | 1.13 | 1.43 | 1.37 | 1.23 |

Table 1: Performance comparison of SMOKE and its improved model with our proposed methods on the KITTI val 3D and BEV detection benchmark. Both metrics are evaluated by AP11 at 0.7 IoU threshold. We choose the SMOKE with ResNet-18 backbone as baseline.

Our proposed Lite-FPN and attention loss are generic modules that can be integrated into most CenterNet-like models.
We choose SMOKE and CenterNet as two baselines, which are both primarily demonstrated on KITTI. To equally compare our approach with two baselines, KITTI is adopted as the main benchmark. Also, most popular methods like D4LCN(2020) and Ground-aware(2021) conduct experiments mainly on KITTI. Therefore, KITTI is adopted heavily in our experiments. It is absolutely necessary to evaluate our approach on the newer and bigger dataset such as nuScenes and Waymo. Unfortunately, the related results of baselines have not been reported yet, which is not suitable for a fair comparison. However, we would test our algorithm on more datasets in our future work.

---

