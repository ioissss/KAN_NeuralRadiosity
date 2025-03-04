# KAN_NeuralRadiosity

neural radiosity 是利用 MLP 重建场景辐射场的手段，探索了一下很火的 KAN 用来替换 MLP 效果怎么样。

前不久，麻省理工学院发表的一篇名为《KAN：Kolmogorov-Arnold Networks》的论文在机器学习社区中引起了轰动，这个创新框架被称为 Kolmogorov-Arnold 网络（KAN）。它为神经网络提供了一个全新的视角，并提出了一种可能的替代传统的多层感知机（MLP）神经网络的新方案。

正好我来试一下这个 KAN 有多牛。

---

## IDEA

neural radiosity 的做法可以看看原文 《Neural radiosity》，其实就是用 MLP 去拟合全局的 radiance field，论文有开源代码，我在其基础上进一步将 MLP 换成了 KAN 来实现。对比中会提到LHand重建和RHand重建，LHand重建指的是，
直接在第一个交点上预测出射radiance，RHand重建指的是，第一个交点上，采样若干方向，预测这些方向的radiance，再算一下积分求出射radiance。

---
---

## 对比：重建质量

| MLP Lhand 重建 | KAN Lhand 重建 |
| --- | --- |
| ![MLP Lhand 重建效果](https://github.com/user-attachments/assets/2b67d3ab-e409-447d-a4b5-dc629e77c612) | ![KAN Lhand 重建效果](https://github.com/user-attachments/assets/0aaf7c9f-924e-4243-8105-29da0d9dd900) |

**图注：**
- **左图**：MLP Lhand 重建效果。
- **右图**：KAN Lhand 重建效果。

| MLP Rhand 重建 | KAN Rhand 重建 |
| --- | --- |
| ![MLP Rhand 重建效果](https://github.com/user-attachments/assets/802b871a-b627-4476-9127-53779bb8cb96) | ![KAN Rhand 重建效果](https://github.com/user-attachments/assets/510f98dd-c42e-4d1a-8ff9-03d86ccad3ab) |

**图注：**
- **左图**：MLP Rhand 重建效果。
- **右图**：KAN Rhand 重建效果。

## 对比：Loss 下降

我们将 MLP 和 KAN 在 loss 下降的效果放在一起进行对比，方便看到它们在训练过程中的差异。

| MLP Loss 下降 | KAN Loss 下降 |
| --- | --- |
| ![MLP Loss 下降](https://github.com/user-attachments/assets/d7968de9-df2b-47c3-902d-d9735721dda9) | ![KAN Loss 下降](https://github.com/user-attachments/assets/9356dd95-c269-4373-9a0d-ca1b6f7d310a) |

**图注：**
- **左图**：MLP 实现的 loss 下降曲线。
- **右图**：KAN 实现的 loss 下降曲线。

## 对比：前向推理时间
| MLP | KAN |
| --- | --- |
| 前向传播一次的平均时间: 3.339421 毫秒 | 前向传播一次的平均时间: 4.938826 毫秒 |

## 总结
KAN的功能和MLP差不太多，但是速度比较慢，而且看LHand的重建质量有明显的问题，说明一些点的误差比较大。
