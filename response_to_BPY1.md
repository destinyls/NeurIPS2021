# Response to reviwer BPY1 

标签： NeurIPS2021_Rebuttal

---
Thanks for your careful reading and constructive comments. we address some concerns and questions as follows.

**Q1**: The idea of using keypoint for 3D object detection is not very novel. This work is upgraded from SMOKE with feature resampling from keypoint information. 

**A1**: Our major novelty is the generic Lite-FPN which is simple yet effective and can be integrated into most CenterNet-like models. Compared with other FPN architecture (such as NAS-FPN, BiFPN, Recursive-FPN ,etc.) that inevitably bring about longer latency and NMS post-process, there are no additional detection heads and the fusion operation of feature maps in Lite-FPN module. Experiments based on SMOKE and CenterNet on KITTI datasets show that our proposed FPN architecture can significantly improve accuracy and meanwhile reduce inference time, which is more appropriate for the edge computing platform in autonomous driving.

---

**Q2**: For autonomous driving purposes, monocular 3D object detection is not very practical since there are usually stereo or other sensors available on an autonomous vehicle. 

**A2**: 
In practical autonomous driving, indeed, most existing solutions adopt stereo or other sensors other than monocular camera. However, the research on monocular 3D object detection is a very hot research direction since 2016. Besides, the newly released Tesla FSD Beta v9 is a pure vision system equipped with eight monocular cameras to capture 360 field of view, which shows that monocular 3D object detection is practical in actual autonomous driving applications.

---

**Q3**: Based on the keypoint, the method can only handle vehicle detection, but other objects are also important, e.g., pedestrians.

**A3**:  Besides the car class, we add the results of pedestrian and cyclist classes in Table 1, to verify the generalization of our proposed solutions. As shown in Table1, our improved SMOKE achieves the performance improvements of 3.38%, 2.75% and 0.86% for car, pedestrian and cyclist respectively on moderate level over the baseline, which explains the generalization of our proposed solutions on different categories.

| Category | Method | $E_{3D}$ | $M_{3D}$ | $H_{3D}$ | $E_{bv}$ | $M_{bv}$ | $H_{bv}$ |
|:---:|:---:|:---:|:---:|:---:| :---:|:---:|:---:|
| Car |  SMOKE  |  12.11 | 10.64 | 10.23 | 16.07 | 13.19 | 11.68 |
| Ped |  SMOKE  |  3.32 | 3.09 | 2.88 | 4.48 | 4.38 | 4.20 |
| Cyc |  SMOKE  |  0.30 | 0.27 | 0.27 | 0.41 | 0.38 | 0.37 |
| Car | Ours-SMOKE |  17.04 | 14.02 | 12.23 | 23.85 | 19.68 | 16.91 |
| Ped | Ours-SMOKE |  6.46 | 5.84 | 5.86 | 6.77 | 6.51 | 6.05 |
| Cyc | Ours-SMOKE |  1.14 | 1.13 | 1.13 | 1.43 | 1.37 | 1.23 |

Table 1: Performance comparison of SMOKE and its improved model with our proposed methods on the KITTI val 3D and BEV detection benchmark. Both metrics are evaluated by AP11 at 0.7 IoU threshold. We choose the SMOKE with ResNet-18 backbone as baseline.