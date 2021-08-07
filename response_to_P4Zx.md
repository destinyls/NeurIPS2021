# Response to reviwer P4Zx 

标签： NeurIPS2021_Rebuttal

---

### **Response to reviewer P4Zx**
**Q.1**: 
The experimental section of the paper is considering a single dataset (KITTI) and a single class (Cars), which is not sufficient.
a). Considering that newer and bigger datasets for the task have been released. 
b). It's uncertain that the proposed solutions generalize besides the car class on the KITTI dataset.
-- from weakness a)

**A.1**:
a). There is only the official evaluation on KITTI dataset available in SMOKE and CenterNet paper. Consequently, it is more reasonable and authoritative to quantify the performance improvement of our solutions on KITTI dataset. Although the majority of Mono3D related papers only provide the evaluation on KITTI, verifying the generalization of proposed methods on nuScenes and Waymo dataset is absolutely significant experiments, which should be the next thing on the agenda.

b). For pedestrian and cyclist cotegories, the performance of the baseline model with DLA-34 backbone on KITTI validation and test set is unavailability in both KITTI leaderboard and SMOKE paper. Consequently, we choose the SMOKE with ResNet-18 backbone as baseline. As shown in Table. 1, our improved SMOKE achieves the performance improvements of 3.38%, 2.75% and 0.86% for car, pedestrian and cyclist respectively on moderate level over the baseline, which verifies the generalization of our proposed solutions on different categories.
| Category | Method | $E_{3D}$ | $M_{3D}$ | $H_{3D}$ | $E_{bv}$ | $M_{bv}$ | $H_{bv}$ |
|:---:|:---:|:---:|:---:|:---:| :---:|:---:|:---:|
| Car |  SMOKE  |  12.11 | 10.64 | 10.23 | 16.07 | 13.19 | 11.68 |
| Ped |  SMOKE  |  3.32 | 3.09 | 2.88 | 4.48 | 4.38 | 4.20 |
| Cyc |  SMOKE  |  0.30 | 0.27 | 0.27 | 0.41 | 0.38 | 0.37 |
| Car | Ours-SMOKE |  17.04 | 14.02 | 12.23 | 23.85 | 19.68 | 16.91 |
| Ped | Ours-SMOKE |  6.46 | 5.84 | 5.86 | 6.77 | 6.51 | 6.05 |
| Cyc | Ours-SMOKE |  1.14 | 1.13 | 1.13 | 1.43 | 1.37 | 1.23 |

Table 1: Performance comparison of SMOKE and its improved model with our proposed methods on the KITTI val 3D and BEV detection benchmark. Both metrics are evaluated by $AP_{11}$ at 0.7 IoU threshold. All the SMOKE performance on car, pedestrian and cyclist are evaluated on the same model, and so is the improved counterparts.

------

**Q.2**: In ablation studies, it is interesting to have in Tab.4 a line with only Attention Loss enabled.
-- from weakness b) 

**A.2**: As shown in Table. 2, The results of “+Lite-FPN” outperform the baseline by 1.84% in $AP_{3D}$ and 3.26% in $AP_{BEV}$ on moderate level. The results of “+Attention Loss” exceed the baseline by 2.29% in $AP_{3D}$ and 5.06% in $AP_{BEV}$ on moderate level. The combination of both Lite-FPN and attention loss improves the performance of baseline by 3.34% and 5.61% in terms of $AP_{3D}$ and $AP_{BEV}$ on moderate level respectively.
| Lite-FPN | Attention Loss | FPS | $E_{3D}$ | $M_{3D}$ | $H_{3D}$ | $E_{bv}$ | $M_{bv}$ | $H_{bv}$ |
|:---:|:---:|:---:|:---:|:---:|:---:| :---:|:---:|:---:|
|     |      | 40.32 | 14.76 | 12.85 | 11.50 | 19.99 | 15.61 | 15.28 |
|$\sqrt{}$|  | 42.37 | 17.37 | 14.69 | 14.09 | 22.42 | 18.87 | 16.48 |
|  |$\sqrt{}$| 40.32 | 17.74  | 15.14  | 14.18  | 24.83  | 20.67  |  17.32 |
|$\sqrt{}$|$\sqrt{}$|**42.37**|**19.31**|**16.19**|**15.47**|**25.45**|**21.22**|**17.91**|

Table 2. Ablation study of SMOKE with different configurations on the KITTI datasets using val split. We use DLA-34 as the backbone. Both metrics on Car category are evaluated by $AP_{11}$ at 0.7 IoU threshold. The frame rate is measured on a RTX 2080Ti and the best results are highlighted in bold type.

------

**Q.3**: A proper study to show a comparison between different methods polling from a different set of layers multi-scale features is necessary.
-- from weakness b) 

**A.3**:
| $f_4$ | $f_8$ | $f_{16}$ | $E_{3D}$ | $M_{3D}$ | $H_{3D}$ | $E_{bv}$ | $M_{bv}$ | $H_{bv}$ |
|:---:|:---:|:---:|:---:|:---:|:---:| :---:|:---:|:---:|
| $\sqrt{}$ |      |    |  |  |  |  |  |  |
|$\sqrt{}$| $\sqrt{}$ | |  |  |  |  |  |  |
| $\sqrt{}$ | | $\sqrt{}$ |  |  |  |  |  |  |
|$\sqrt{}$|$\sqrt{}$|$\sqrt{}$| |  |  |  |  |  |

Table 3.

------


**Q.4**: why the $IoU_{3D}$ part is needed in Eq.5. If the boxes have a good IoU their regression loss $l_{reg_i}$ will be low already, so it is not clear why it should be downweighted further. In the paper, there is no study on the impact of $\beta$ on the final performance which would help solve my doubts expressed above.  
-- from weakness c)

**A.4**:
|Method | $\beta$ | $E_{3D}$ | $M_{3D}$ | $H_{3D}$ | $E_{bv}$ | $M_{bv}$ | $H_{bv}$ |
|:---:|:---:|:---:|:---:|:---:| :---:|:---:|:---:|
|  Baseline  |  -  |  |  |  |  |  |  |
|  +Attention Loss |  0.5 |  |  |  |  |  |  |
|  +Attention Loss |  1   |  |  |  |  |  |  |
|  +Attention Loss |  2   |  |  |  |  |  |  |
|  +Attention Loss |  4   |  |  |  |  |  |  |

Table 4.

------

**Q.5**: when pooling K locations during training/inference, are those obtained after a NMS stage based on max pooling as in the original CenterNet? Moreover, what is the value of K used for the experiments?
-- from weakness d)

**A.5**:
In the training phase, the ground truth keypoints is defined as the K locations. In the inference phase, the K locations is acquired through a topK operation on the heatmap processed by max pooling.
K is set to 30 during training and 50 when making inference, which is positively correlated to the number of object per image.

------


**Q.6**: The discussion about the limitations is limited to a single sentence in Section 5. When building on top of existing works (CenterNet/SMOKE) I would like to see both wins (e.g., the sample in Fig. 4) as well as Losses, if any (i.e., samples where the proposed model performs worse than the original CenterNet or SMOKE). 
-- from weakness e)

**A.6**:

------

**Q.7**: Why is the $\times N$ in eq.5 needed?  In Eq.2 $L_{reg}$ is normalized by the number of keypoints while in Eq.4 it is not. Wouldn’t multiply by $N$ the weights scale the overall loss by $N$ as well?
-- from questions 1.

**A.7**:

------

**Q.8**: Are gradients back propagated through the weight computation in Eq. 4 and 5? This will change the interpretation of the loss function as it will have a direct effect also on the keypoints prediction branch. If so please state this in the final text.
-- from questions 2.

**A.8**:

------


**Q.9**: Is $N$ in Eq. 1, 2 and 5 the number of keypoints per image or per batch of images? I’m assuming the former as the rest of the loss discussion seems to refer to a single image, but in the text $N$ is presented as “the total number of keypoints per batch images”.
-- from questions 3.

**A.9**:

------

**Q.10**: I would add to Table 3 also the results from the best performing model on the validation set: Ours-SMOKE with DLA-34.
-- from suggestions.

**A.10**:

------






