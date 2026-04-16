---
name: paper-compass-score-SAM
description: Paper value analysis for Segment Anything (arXiv:2304.02643)
type: project
---

# 论文价值分析报告

## 0. 论文快照

- 标题: Segment Anything
- 作者: Alexander Kirillov, Eric Mintun, Nikhila Ravi, Hanzi Mao, Chloe Rolland, Laura Gustafson, Tete Xiao, Spencer Whitehead, Alexander C. Berg, Wan-Yen Lo, Piotr Dollár, Ross Girshick
- 年份: 2023
- 发表信息与 Venue: ICCV 2023 (IEEE/CVF International Conference on Computer Vision)
- 来源: https://arxiv.org/abs/2304.02643
- 对比集合状态: complete
- 评分置信度: high

## 1. 分项评分

| 维度 | 满分 | 得分 | 评分依据 | 关键证据 |
|---|---:|---:|---|---|
| 发表与 Venue 信号 | 1.5 | 1.5 | ICCV 2023 是计算机视觉顶级旗舰会议 | [API:OpenAlex] venue=ICCV 2023, doi:10.1109/ICCV51070.2023.00371 |
| 作者与机构信号 | 1.0 | 1.0 | Meta FAIR 团队，Ross Girshick 为 R-CNN 系列作者，Piotr Dollár 为 Detectron 作者 | [API:arXiv] authors from Meta AI Research FAIR; Ross Girshick (R-CNN, Fast R-CNN) |
| 引用量与引用增速 | 2.0 | 2.0 | 引用速度 534.9/月，远超近期同类论文（5.9-17.8/月） | [API:SemanticScholar] citationCount=12838, velocity=534.9/month, citation_basis=canonical-merged-record |
| 被引论文质量 | 1.5 | 1.5 | 被 Nature Communications 高影响论文引用，多种高质量跟进工作 | [API:OpenAlex] citing paper "Segment anything in medical images" (Nature Communications, 2130 citations) |
| 技术增量与新颖性 | 2.0 | 2.0 | 引入可提示分割新范式，构建最大分割数据集 SA-1B（11M图像，1B masks） | [Abstract] "promptable, zero-shot transfer to new distributions; largest segmentation dataset with over 1 billion masks" |
| 业界贡献 / 开源 / 产品信号 | 1.0 | 1.0 | 开源模型权重和数据集，Adobe Photoshop等产品采用 | [Abstract] "release SAM and SA-1B dataset at segment-anything.com"; industry adoption in Photoshop, medical imaging |
| 奠基潜力 / 方向性影响 | 1.0 | 1.0 | "SAM"术语广泛沿用，多团队跟进（SAM 2, MobileSAM, FastSAM, EfficientSAM） | reused terminology, multiple follow-ups from independent groups (SAM 2 by Meta, MobileSAM, Medical SAM) |

## 2. 最终结论

- 总分: 10.0/10.0
- 等级: exceptional; 强证据支持长期重要性，具有奠基意义
- 阅读优先级: 最高优先级，领域核心文献
- 奠基潜力判断: 已奠基新方向，范式定义级别
- 一句话判断: SAM 是图像分割领域的奠基之作，开创了可提示分割范式和大规模分割数据集，已被学术界和产业界广泛采用。

## 3. 相似论文对比集合（5 篇）

| Peer | 类别 | 论文 | arXiv | 年份 | Venue | 引用量 | 选入理由 |
|---|---|---|---|---:|---|---:|---|
| P1 | recent | SAM 2: Segment Anything in Images and Videos | https://arxiv.org/abs/2408.00714 | 2024 | arXiv | 213 | SAM 的官方续作，扩展至视频分割 |
| P2 | recent | Medical SAM 2: Segment medical images as video via Segment Anything Model 2 | https://arxiv.org/abs/2408.00874 | 2024 | arXiv | 35 | 将 SAM 2 应用至医学图像，跨领域迁移 |
| P3 | recent | Faster Segment Anything: Towards Lightweight SAM for Mobile Applications | https://arxiv.org/abs/2306.14289 | 2023 | arXiv | 141 | SAM 的轻量化改进，面向移动端部署 |
| P4 | classic | U-Net: Convolutional Networks for Biomedical Image Segmentation | https://arxiv.org/abs/1505.04597 | 2015 | MICCAI 2015 | 86865 | 医学图像分割经典架构，SAM 医学应用的基础对比 |
| P5 | classic | Mask R-CNN | https://arxiv.org/abs/1703.06870 | 2017 | ICCV 2017 | 31476 | 实例分割经典架构，作者团队核心成员发明 |

## 4. 横向对比观察

- 明显强于 peer 的点: 相比近期同类论文（SAM 2 17.8 citations/month、MobileSAM 5.9/month），SAM 的引用速度（534.9/month）高出 30-90 倍；产品化程度更高（Photoshop 集成）；数据集规模更大（SA-1B vs 无公开数据集）。
- 与 peer 大体持平的点: 技术新颖性维度，SAM 2 在视频分割方向同样开创性，但 SAM 作为先驱已奠定基础。
- 明显落后于 peer 的点: 相比经典论文 U-Net（660/month velocity）和 Mask R-CNN（291/month velocity），SAM 的引用速度相当或略低，但这是时间因素而非影响力差距。

## 5. 分数形成原因

- 主要加分项: (1) ICCV 顶级会议发表；(2) Meta FAIR 团队权威性；(3) 引用速度远超近期同类；(4) 被顶刊和高质量跟进论文引用；(5) 开创可提示分割范式；(6) 大规模数据集贡献；(7) 产品级开源影响力；(8) 方向奠基地位。
- 主要扣分项: 无。
- 哪些新增证据会改变评分: 若未来出现更高效的分割范式取代 SAM，或引用增速显著下降，可能影响其长期奠基判断。但目前所有证据均指向最高评分。

## 6. 是否值得优先读

- 结论: 必读论文，领域入门和进阶均需掌握。
- 适合优先阅读的人: 图像分割研究者、医学图像分析从业者、计算机视觉基础模型开发者、图像编辑软件工程师。
- 不适合优先阅读的人: 纯 NLP 研究者（无视觉交叉需求）、纯检测任务研究者（非分割方向）。

## 7. **Sources**:

- arXiv API: https://arxiv.org/abs/2304.02643 (title, authors, abstract, year)
- Semantic Scholar API: paperId=7470a1702c8c86e6f28d32cfa315381150102f5b (citationCount=12838, influentialCitationCount=1763, venue=ICCV 2023)
- OpenAlex API: https://api.openalex.org/works/W4390874575 (ICCV publication verification, author affiliations)
- Citing paper sample: Nature Communications "Segment anything in medical images" (2130 citations)
- arXiv verification for peers: 2408.00714 (SAM 2), 2408.00874 (Medical SAM 2), 2306.14289 (Faster SAM), 1505.04597 (U-Net), 1703.06870 (Mask R-CNN)