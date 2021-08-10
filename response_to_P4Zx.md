# Response to reviwer P4Zx 

标签： NeurIPS2021_Rebuttal

---
**Q1**: The experimental section of the paper is considering a single dataset (KITTI) and a single class (Cars), which is not sufficient.

a). Considering that newer and bigger datasets for the task have been released.

b). It's uncertain that the proposed solutions generalize besides the car class on the KITTI dataset.

**A1**: 

a). Our proposed Lite-FPN and attention loss are generic modules that can be integrated into most CenterNet-like models.
We choose SMOKE and CenterNet as two baselines, which are both primarily demonstrated on KITTI. To equally compare our approach with two baselines, KITTI is adopted as the main benchmark. Also, most popular methods like D4LCN(2020) and Ground-aware(2021) conduct experiments mainly on KITTI. Therefore, KITTI is adopted heavily in our experiments. It is absolutely necessary to evaluate our approach on the newer and bigger dataset such as nuScenes and Waymo. Unfortunately, the related results of baselines have not been reported yet, which is not suitable for a fair comparison. However, we would test our algorithm on more datasets in our future work.

b). Besides the car class, we add the results of pedestrian and cyclist classes in Table 1 to verify the generalization of our proposed solutions. As shown in Table1, our improved SMOKE achieves the performance improvements of 3.38%, 2.75% and 0.86% for car, pedestrian and cyclist respectively on moderate level over the baseline, which verifies the generalization of our proposed solutions on different categories.

| Category | Method | $E_{3D}$ | $M_{3D}$ | $H_{3D}$ | $E_{bv}$ | $M_{bv}$ | $H_{bv}$ |
|:---:|:---:|:---:|:---:|:---:| :---:|:---:|:---:|
| Car |  SMOKE  |  12.11 | 10.64 | 10.23 | 16.07 | 13.19 | 11.68 |
| Ped |  SMOKE  |  3.32 | 3.09 | 2.88 | 4.48 | 4.38 | 4.20 |
| Cyc |  SMOKE  |  0.30 | 0.27 | 0.27 | 0.41 | 0.38 | 0.37 |
| Car | Ours-SMOKE |  17.04 | 14.02 | 12.23 | 23.85 | 19.68 | 16.91 |
| Ped | Ours-SMOKE |  6.46 | 5.84 | 5.86 | 6.77 | 6.51 | 6.05 |
| Cyc | Ours-SMOKE |  1.14 | 1.13 | 1.13 | 1.43 | 1.37 | 1.23 |

Table 1: Performance comparison of SMOKE and its improved model with our proposed methods on the KITTI val 3D and BEV detection benchmark. Both metrics are evaluated by $AP_{11}$ at 0.7 IoU threshold. We choose the SMOKE with ResNet-18 backbone as baseline.

---

**Q2**:  In ablation studies, it is interesting to have in Tab.4 a line with only Attention Loss enabled.

**A2**: We add the results with only attention loss enabled in Table 2. As shown in Table. 2, the results of “+Lite-FPN” outperform the baseline by 1.84% in $AP_{3D}$ and 3.26% in $AP_{BEV}$ on moderate level. The results of “+Attention Loss” exceed the baseline by 1.98% in $AP_{3D}$ and 4.05% in $AP_{BEV}$ on moderate level. The combination of both Lite-FPN and attention loss improves the baseline performance by 3.34% and 5.61% in terms of $AP_{3D}$ and $AP_{BEV}$ on moderate level respectively.

| Lite-FPN | Attention Loss | FPS | $E_{3D}$ | $M_{3D}$ | $H_{3D}$ | $E_{bv}$ | $M_{bv}$ | $H_{bv}$ |
|:---:|:---:|:---:|:---:|:---:|:---:| :---:|:---:|:---:|
|     |      | 40.32 | 14.76 | 12.85 | 11.50 | 19.99 | 15.61 | 15.28 |
|$\sqrt{}$|  | 42.37 | 17.37 | 14.69 | 14.09 | 22.42 | 18.87 | 16.48 |
|  |$\sqrt{}$| 40.32 | 17.32  | 14.83  | 14.05  | 23.89  | 19.66  |  17.19 |
|$\sqrt{}$|$\sqrt{}$|**42.37**|**19.31**|**16.19**|**15.47**|**25.45**|**21.22**|**17.91**|

Table 2. Ablation study of SMOKE with different configurations on the KITTI datasets using val split. We use DLA-34 as the backbone. Both metrics on Car category are evaluated by AP11at 0.7 IoU threshold.

---

**Q3**: The feature pooling schema reminds me of ROI-pooling in two stage object detection frameworks, but unfortunately there is no study on the effect of how many levels of feature representations to pool (i.e, 1, 2 or 3).  A proper study to show a comparison between different methods polling from a different set of layers multi-scale features is necessary.

**A3**: We add the ablation experiments of the Lite-FPN with different sets of layers in Table 3. As shown in Table 3, each of the down-sampled feature maps (x4, x8, and x16) is effective for bounding box regression. Combining all three different feature layers in Lite-FPN, we manage to achieve the optimum performance.

|Method|$f_4$ | $f_8$ | $f_{16}$ | $E_{3D}$ | $M_{3D}$ | $H_{3D}$ | $E_{bv}$ | $M_{bv}$ | $H_{bv}$ |
|:---:|:---:|:---:|:---:|:---:|:---:|:---:| :---:|:---:|:---:|
| SMOKE  |$\sqrt{}$ |               |             |12.11 | 10.64 | 10.23 | 10.91 | 13.95 | 12.58 |
| Lite-FPN |$\sqrt{}$ |               |             |10.31| 10.17 | 9.86 | 12.74 | 11.43 | 11.15 |
| Lite-FPN |              | $\sqrt{}$ |             |13.99 | 11.76 | 11.39 | 19.02 | 15.79 | 11.39 |
| Lite-FPN |$\sqrt{}$|$\sqrt{}$|               |14.20 | 11.89 | 11.51 | 19.38 | **16.20** | 14.53 |
| Lite-FPN |              |             | $\sqrt{}$ | 14.01 | 11.94 | 11.54 | 18.68 | 15.88 | 14.42 |
| Lite-FPN | $\sqrt{}$ |            | $\sqrt{}$ | 14.03 | 12.05 | **11.56** | 18.78 | 15.97 | 14.57 |
| Lite-FPN |                |$\sqrt{}$|$\sqrt{}$| 14.85 | 12.83 | 11.51 | 19.99 | 15.91 | **14.94** |
| Lite-FPN |$\sqrt{}$|$\sqrt{}$|$\sqrt{}$| **15.27** | **12.93** | 11.48 | **20.96** | 16.11 | 14.76 |

Table 3. Ablation study of Lite-FPN with different layers configurations on KITTI val 3D and BEV detection benchmark. Both metrics on Car category are evaluated by $AP_{11}$ at 0.7 IoU threshold. We use the origin SMOKE with ResNet-18 backbone as the baseline.

---

**Q4**: Why the $IoU_{3D}$ part is needed in Eq.5. If the boxes have a good IoU, their regression loss $l_{reg_{i}}$ will be low already, so it is not clear why it should be downweighted further. In the paper, there is no study on the impact of β on the final performance which would help solve my doubts expressed above. 

**A4**: For the purpose of certifying the effect of $IoU_{3D}$ in Eq.5, we carry out the ablation experiments of attention loss with different values of β. As shown in Table 4, inappropriate proportion of term $(1 - IoU_{3D})$ in Eq.5, too big or too little, fails to achieve great performance. The trade-off between confidence score and localization precision when β is 0.5 get relatively best performance.

|Method|$\beta$ | $E_{3D}$ | $M_{3D}$ | $H_{3D}$ | $E_{bv}$ | $M_{bv}$ | $H_{bv}$ |
|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
|SMOKE| / | 14.76 | 12.85 | 11.50 | 19.99 | 15.61  | 15.28  |
|Attention Loss| 0 |  16.25 | 14.18 | 13.50 |  19.50 | 18.28  | 16.13  |
|Attention Loss|  0.5 | **17.32** | **14.83** | **14.05** | **23.89**  | **19.66**  | **17.19**  |
|Attention Loss|  1.0 | 16.93 | 14.19 | 13.27 | 23.02  | 18.67  | 16.19  |
|Attention Loss|  2.0 |  16.60| 14.15 | 13.32 | 22.11  | 18.25  | 15.60  |
|Attention Loss|  infinite | 16.32 | 13.72 | 12.97 | 22.34  | 18.11  | 15.43  |

Table 4. Ablation study of the impact of $IoU_{3D}$ part in attention loss. We use the origin SMOKE with DLA-34 backbone as the baseline. Experiment results are evaluated on the KITTI dataset using val split. Both metrics for car category are evaluated by $AP_{11}$ at 0.7 IoU threshold.

---

**Q5**: Some implementation details of the method are currently missing. For example: when pooling K locations during training/inference, are those obtained after a NMS stage based on max pooling as in the original CenterNet? If not, are the authors doing anything to avoid pooling overlapping locations nearby a local maxima? Moreover, what is the value of K used for the experiments?

**A5**: In the training stage, the ground truth keypoints are defined as the K locations. In the inference phase, the K locations are acquired through a top-K operation on the heatmap processed by max pooling. K is positively correlated to the number of objects per image, and its actual value is 30 during training and 50 when making inference.

---

**Q6**: Why is the $×N$ in Eq.5 needed?  In Eq.2, $L_{reg}$ is normalized by the number of keypoints while in Eq.4 it is not. Wouldn’t multiply by $N$ the weights scale the overall loss by $N$ as well?

**A6**: It is necessary to normalize the $L_{reg}$ by the number of keypoints N in both Eq. 2 and Eq. 4.
The simplified form of Eq. 5 can be expressed as $w_i=Softmax(P_i+ \beta ×(1 - IoU_{3D}))×N$. And $×N$ ensure $\sum_{i=1}^{N}w_i = N$, which is able to maintain the same hyper-parameters λ in Eq. 3 as before.

---

**Q7**: Are gradients back propagated through the weight computation in Eq. 4 and 5? This will change the interpretation of the loss function as it will have a direct effect also on the keypoints prediction branch. If so please state this in the final text.

**A7**: Yes, gradients will be backpropagated through the weight computation in Eq. 4 and 5. We will state this point in the final submission.

---

**Q8**: Is $N$ in Eq. 1, 2 and 5 the number of keypoints per image or per batch of images? I’m assuming the former as the rest of the loss discussion seems to refer to a single image, but in the text $N$ is presented as “the total number of keypoints per batch images”.

**A8**: $N$ in Eq. 1, 2, 4, and 5 represent the number of keypoints per image instead of per batch image. We will amend the error in the final submission.




