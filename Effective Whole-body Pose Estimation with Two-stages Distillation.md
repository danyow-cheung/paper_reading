# Effective Whole-body Pose Estimation with Two-stages Distillation

>  https://arxiv.org/pdf/2307.15880v1.pdf

## Abstract 

全身姿态估计器,



第一階段蒸餾設計了權重衰減策略，同時利用教師的中間特徵和具有可見和不可見關鍵點的最終邏輯來從頭開始監督學生。

第二階段蒸餾學生模型自己，為提高表現。



## Introduction

人體姿勢估計技術的進一步進步對於充分釋放用戶驅動的內容創建的潛力至關重要



整體姿勢估計更大的挑戰在於

1. 用於細粒度關鍵點定位的人體層次結構；
2. 低分辨率的手部和臉部
3. 複雜的人體部位匹配，在一張圖片中有多個人的情況下，特別是阻擋和複雜的手部姿勢
4. 數據限制，特別是全身圖片中，變化多樣的手部和頭部姿勢



在進入模型先，我們用知識蒸餾的方法來實現。

使用**RTMPose**作為basic model



**在第一階段的知識蒸餾中，我們使用RTMPose-x的中間層和最終層的最後的Logits來指導學生（RTMPose-l）模型。**

1. 我們使用看得見和看不見的關鍵點作為final logits ，這樣可以讓學生模型學習的更快，更容易
2. 同時，我們採用權重衰減策略來提高效率，在整個訓練階段逐漸減少蒸餾的權重。

**在第二階段，提出頭部感知的自我KD（a head-aware self-kd） 去提高head的能力**

1. 選擇兩個模型，一個老師，一個學生。學生模型的backbone是凍結的，只有頭部會隨著logits更新
2. 值得注意的是，這種即插即用的方法允許
   學生可以用 20% 的訓練時間獲得更好的結果，無論是否從頭開始進行蒸餾訓練，並且可用於任何密集的預測頭



## Related work

### 2d Whole-body Pose Estimation

提及了一些框架，比如說

openpose，Mediapipe，ZoomNet，ZoomNAS，TCFormer，

### Knowledge Distillation

知識蒸餾，也叫做基於Logit的蒸餾，

從基於邏輯的蒸餾到基於特徵的蒸餾，知識從
中間層  並將蒸餾擴展到各種任務，包括檢測 、分割 生成 等

2D 全身姿態估計是 3D 姿態估計的基本任務，比僅身體姿態估計更全面。



DWPose首要就要探索有效率的知識蒸餾策略





## Method

<img src = 'paper_imgs/Two-stages Pose Distillation.png'>

### The First-stage distillation

第一階段的蒸餾強迫學生從老師的特徵Ft和邏輯Ti

#### Feature-based distillation

強迫學生從骨幹直接模仿老師的層。



使用MSE損失區計算學生特徵Fs和老師特徵Ft的距離



#### Logit-based distillation

RTMPose [18] 使用基於 SimCC [22] 的算法預測姿態關鍵點，該算法將關鍵點定位視為水平和垂直坐標的分類任務



#### Weight-decay strategy for distillation

學生的總共loss表示
$$
L = L_{ori}+åL_{feature}+ßL_{logit}
$$
我們利用時間函數r(t)來實現該策略，如下：
$$
r(t) = 1-(t-1)/t_{max}
$$
t表示當前epoch，

第一階段的loss可以標示為
$$
L_{s1} = L_{ori}+r(t)*åL_{feature}+r(t)*ßL_{logit}
$$




### The second-stage distillation

我們嘗試利用訓練有素的學生模型進行自學，以獲得更好的表現。 這樣，無論是否從頭開始進行蒸餾訓練，它都可以為學生帶來進步。



學生模型首先建立一個訓練的backbone和未訓練的head。老師也是同樣的訓練的backbone和head模型



在訓練過程中，凍結學生的backbone，更新學生的head。

Because the teacher and the student have the same architecture, we only need to extract the feature from the backbone once

因為老師和學生具有相同的架構，所以我們只需要從主幹中提取一次特徵



Then, the feature is fed into the teacher’s trained head and the student’s untrained head to get the logits Ti and Si , respectively. Following the form in Eq. 3, we train the student with Llogit for the second-stage distillation. It’s worth noting that we drop the original loss Lori, which is calculated with label value. Using γ to denote the hyper-parameter for loss scale, the final loss for the second-stage distillation can be formulated as:

然後，將該特徵輸入教師訓練過的大腦和學生未訓練的大腦中，分別得到 Logits Ti 和 Si 。 下列的
方程中的形式 3、我們用Llogit訓練學生進行第二階段蒸餾。 值得注意的是，我們去掉了原始損失 Lori，它是用標籤值計算的。 使用γ表示損失尺度的超參數，第二階段蒸餾的最終損失可以表示為：
$$
L_{s2} = yL_{logit}
$$


## Experiments

可以忽略了

