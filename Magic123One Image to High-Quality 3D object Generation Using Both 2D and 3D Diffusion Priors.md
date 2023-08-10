# Magic123:One Image to High-Quality 3D object Generation Using Both 2D and 3D Diffusion Priors

> https://arxiv.org/pdf/2306.17843v2.pdf

## Abstract

一種兩階段從粗到細的方法，使用 2D 和 3D 先驗從單個未擺出的圖像生成高質量、有紋理的 3D 網格



在第一階段，我們優化神經輻射場以產生粗略的幾何形狀。

在第二階段，我們採用內存高效的可微網格表示來生成具有視覺吸引力紋理的高分辨率網格。

In both stages, the 3D content is learned through reference view supervision and novel views guided by a combination of 2D and 3D diffusion priors

在這兩個階段中，3D 內容都是通過參考視圖監督和由 2D 和 3D 擴散先驗相結合引導的新穎視圖來學習的



## Introduction

相反，從計算機視覺的角度來看，儘管經過了數十年的探索和發展，從未擺出的圖像進行 3D 重建的任務（包括幾何和紋理的創建）仍然是一個未解決的不適定問題



我們將人類和機器之間 3D 重建能力的巨大差異歸因於兩個主要因素

- 大規模 3D 數據集的缺陷阻礙了 3D 幾何的大規模學習，

- 處理 3D 數據時細節水平和計算資源之間的權衡。



DreamFusion [62] 是這種基於 2D 先驗的文本到 3D 生成方法的開拓者



我們分析了 2D 和 3D 先驗的行為，發現它們都有優點和缺點。 2D 先驗對於 3D 生成表現出令人印象深刻的泛化能力，這是 3D 先驗無法實現的

然而，由於 3D 知識有限，僅依賴 2D 先驗的方法不可避免地會損害 3D 保真度和一致性。





在本文中，我們提倡同時使用這兩種先驗來指導圖像到 3D 生成中的新穎視圖，而不是僅僅依賴 2D 或 3D 先驗。



提出Magic123通過two-stage coarse-to-fine 

在coarse階段，**我們優化神經輻射場（NeRF）**

為了提升3D內容的質量，來到第二屆階段，

**採用內存高效且紋理分解的 SDF-Mesh 混合表示，稱為 Deep Marching Tetrahedra (DMTet)**





## Methodology

<img src = 'paper_imgs/Magic123.png'>

