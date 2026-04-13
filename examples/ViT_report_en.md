# Paper Compass Report

## 0. Paper Snapshot

- Title: An Image is Worth 16x16 Words: Transformers for Image Recognition at Scale
- Authors: Alexey Dosovitskiy, Lucas Beyer, Alexander Kolesnikov, Dirk Weissenborn, Xiaohua Zhai, Thomas Unterthiner, Mostafa Dehghani, Matthias Minderer, Georg Heigold, Sylvain Gelly, Jakob Uszkoreit, Neil Houlsby (Google Research, Brain Team)
- Year: 2020 (arXiv), 2021 (ICLR published)
- Publication & Venue: ICLR 2021 | JCR Quartile: N/A | CCF Rank: A
- Source: https://arxiv.org/html/2010.11929
- **Impact**: 70,000+ citations (as of 2024), seminal paper establishing Vision Transformer in computer vision, ICLR 2021 Oral Presentation

## 1. Must-Learn Prerequisites (Ordered)

| Order | Concept | Why Needed | Where Used In Paper | Evidence | Difficulty | Minimum Goal | Estimated Time |
|---|---|---|---|---|---|---|---|
| 1 | Self-Attention Mechanism | ViT's core computational unit, understanding Q/K/V matrix operations is fundamental | Section 3.1 | `[Sec 3.1] "alternating layers of multiheaded self-attention (MSA)"` | ⭐⭐⭐⭐ | Master Q, K, V definitions, understand scaled dot-product attention formula | 2-3h |
| 2 | Transformer Encoder Structure | ViT directly uses standard Transformer Encoder, must understand its complete structure | Section 3.1 | `[Sec 3.1] "provide these sequences as input to a standard Transformer encoder"` | ⭐⭐⭐⭐ | Understand Encoder consists of MSA + MLP + LayerNorm + Residual Connection layers | 2-3h |
| 3 | Position Embedding | Transformer doesn't encode position information, ViT uses learnable position embedding | Section 3.1 | `[Sec 3.1] "We add learnable 1D position embeddings to the embedded patches"` | ⭐⭐⭐ | Understand why position encoding is needed, ViT uses learnable 1D position embedding | 1-2h |
| 4 | BERT [class] Token Concept | ViT borrows BERT's classification token design for final classification output | Section 3.1 | `[Sec 3.1] "similar to BERT's [class] token, we prepend a learnable embedding"` | ⭐⭐ | Understand prepending a special token at sequence start to aggregate global information | 1h |
| 5 | Patch Embedding | ViT splits images into patch sequences, core design for adapting images to Transformer | Section 3.1 | `[Sec 3.1] "We split an image into fixed-size patches, linearly embed each patch"` | ⭐⭐⭐ | Understand how H×W×C image is divided into N P×P patches | 1-2h |
| 6 | CNN Basics and ResNet | Paper's core comparison object, understanding CNN inductive bias explains ViT design motivation | Section 1, 3.1 | `[Sec 1] "CNNs have been the dominant approach"` | ⭐⭐ | Understand convolution operation, pooling, ResNet residual connections | 2h |
| 7 | Transfer Learning | Paper's core training strategy: large-scale pre-training + small dataset fine-tuning | Section 1, 3.2 | `[Sec 1] "pre-trained on large amounts of data and transferred to multiple benchmarks"` | ⭐⭐⭐ | Understand pre-training-fine-tuning paradigm | 2h |
| 8 | Multi-Head Attention | ViT uses multi-head attention to capture features in different subspaces | Appendix A | `[Appendix A] "h=8 parallel attention layers, or heads"` | ⭐⭐⭐ | Understand multi-head attention projects Q/K/V to multiple subspaces, computes in parallel, then concatenates | 1h |

## 2. Bridge Concepts (Optional but Helpful)

| Concept | Role | Where Used | Evidence | Difficulty | Suggested Action |
|---|---|---|---|---|---|
| Inductive Bias | bridge | Section 3.1 | `[Sec 3.1] "ViT has much less image-specific inductive bias than CNNs"` | ⭐⭐⭐ | learn: understand how model's built-in assumptions affect learning |
| LayerNorm | bridge | Section 3.1 | `[Sec 3.1] "LayerNorm (LN) is applied before every block"` | ⭐⭐ | review: Transformer uses LN instead of BatchNorm |
| MLP Block (Feed-Forward) | bridge | Section 3.1 | `[Sec 3.1] "MLP blocks with GELU non-linearity"` | ⭐⭐ | review: understand two-layer fully connected + GELU activation structure |
| Adam Optimizer | bridge | Appendix B.1 | `[Appendix B.1] "Adam with weight decay 0.1"` | ⭐ | skip: training details |
| Hybrid Architecture | bridge | Section 3.1 | `[Sec 3.1] "CNN feature maps can be used as input patches instead"` | ⭐⭐ | optional: CNN+Transformer hybrid architecture |

## 3. Personalized Delta (Based on memory.md)

- Skipped because already mastered:
  - None (memory.md not loaded)

- Kept despite familiarity (paper-specific delta):
  - None

- New high-priority gaps:
  - **Patch Embedding**: ViT's core innovation, adapting images to Transformer
  - **Inductive Bias difference**: Understanding the fundamental distinction between ViT and CNN

## 4. Recommended Learning Resources

### Self-Attention Mechanism

- Paper/Survey:
  - Attention Is All You Need - https://arxiv.org/abs/1706.03762
- Video:
  - The Illustrated Transformer - https://jalammar.github.io/illustrated-transformer/
  - Stanford CS224N Lecture 9: Transformers - https://www.youtube.com/watch?v=5sfB8h9q0L0
- Why this pair:
  - Self-Attention is the absolute prerequisite for understanding ViT, must master Q/K/V matrix operations first

### Transformer Encoder Structure

- Paper/Survey:
  - Attention Is All You Need - https://arxiv.org/abs/1706.03762
- Video:
  - The Illustrated Transformer - https://jalammar.github.io/illustrated-transformer/
- Why this pair:
  - ViT uses standard Transformer Encoder (no Decoder)

### Position Embedding

- Paper:
  - Attention Is All You Need - https://arxiv.org/abs/1706.03762
- Related Paper:
  - BERT: Pre-training of Deep Bidirectional Transformers - https://arxiv.org/abs/1810.04805
- Why this pair:
  - ViT uses learnable position embedding, not sinusoidal

### BERT [class] Token Concept

- Paper:
  - BERT: Pre-training of Deep Bidirectional Transformers - https://arxiv.org/abs/1810.04805
- Why this pair:
  - ViT directly borrows BERT's design, prepending class token at patch sequence start

### Patch Embedding

- Paper:
  - ViT paper Section 3.1 - https://arxiv.org/html/2010.11929
- Why this pair:
  - This is ViT's core innovation for adapting images to Transformer

### CNN Basics and ResNet

- Paper:
  - Deep Residual Learning for Image Recognition - https://arxiv.org/abs/1512.03385
- Video:
  - Andrew Ng CNN course - https://www.coursera.org/learn/convolutional-neural-networks
- Why this pair:
  - Understanding CNN's locality and translation equivariance explains why ViT lacks inductive bias

### Transfer Learning

- Related Paper:
  - Big Transfer (BiT) - https://arxiv.org/abs/1912.11370
- Why this pair:
  - ViT's success depends on large-scale pre-training

## 5. Suggested Study Sequence

1. Stage A (Foundation): Self-Attention → Multi-Head Attention → Transformer Encoder Structure
2. Stage B (Key Components): Position Embedding → BERT [class] Token → Patch Embedding
3. Stage C (Comparative Understanding): CNN Basics and ResNet → Transfer Learning
4. Stage D (Paper Reading): Review ViT paper Section 1 (Introduction) and Section 3.1 (Method)

## 6. 30-Minute Fast Start

Key experimental conclusions: ViT surpasses ResNet on large datasets (JFT-300M), achieving same performance with 1/2 to 1/4 computation of ResNet; CNN's built-in assumptions like locality have advantages on small data but become limitations on large data.

## 7. **Sources**:

- [ViT Paper - arXiv](https://arxiv.org/html/2010.11929)
- [ViT Paper - OpenReview ICLR 2021](https://openreview.net/forum?id=YicbFdNTTy)
- [Attention Is All You Need](https://arxiv.org/abs/1706.03762)
- [BERT Paper](https://arxiv.org/abs/1810.04805)
- [ResNet Paper](https://arxiv.org/abs/1512.03385)
- [The Illustrated Transformer](https://jalammar.github.io/illustrated-transformer/)