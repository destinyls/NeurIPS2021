# Response to reviwer VCNY 

标签： NeurIPS2021_Rebuttal

---

Thanks for your careful reading and valuable comments.

We summarize the innovations in this paper as follows:

[1] We propose a 'plug and play' Lite-FPN module which is simple yet effective and can be integrated into most CenterNet-like models. Compared with other FPN architecture (such as NAS-FPN, BiFPN, Recursive-FPN ,etc.) that inevitably bring about longer latency and NMS post-process, there are no additional detection heads and the fusion operation of feature maps in Lite-FPN. Experiments based on SMOKE and CenterNet on KITTI datasets show that our proposed FPN architecture can significantly improve accuracy and meanwhile reduce inference time, which is more appropriate for the edge computing platform in autonomous driving.

[2] With the proposed attention loss applied in training phase, we solve the misalignment between classification score and localization precision without introducing additional branches.