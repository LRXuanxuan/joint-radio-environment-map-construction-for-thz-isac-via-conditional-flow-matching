# Joint Radio-Environment Map Construction for THz ISAC via Conditional Flow Matching

[![Award](https://img.shields.io/badge/Award-Best%20Student%20Paper-gold?style=flat-square)](#award)
[![Venue](https://img.shields.io/badge/Venue-IEEE%20ICCT%202026-blue?style=flat-square)](#award)
[![Pages](https://img.shields.io/badge/Paper-6%20pages-lightgrey?style=flat-square)](paper/ICCT2026-Joint-Radio-Environment-Map-Construction-THz-ISAC-CFM.pdf)

**🏆 Best Student Paper Award** — 2026 IEEE 26th International Conference on Communication Technology (ICCT 2026), Zhuhai, China, 16–18 October 2026.

**Authors:** Ruixuan Li, Shuai Wang, Lingxiang Li, and Zhi Chen

**Affiliation:** National Key Laboratory of Wireless Communications, University of Electronic Science and Technology of China (UESTC), Chengdu 611731, China

📄 [Paper (PDF, 6 pages)](paper/ICCT2026-Joint-Radio-Environment-Map-Construction-THz-ISAC-CFM.pdf) · 🏆 Best Student Paper Award · 📌 ICCT 2026, Zhuhai, China

---

## Abstract

Terahertz (THz) integrated sensing and communication (ISAC) requires accurate radio-environment awareness despite severe blockage and sparse measurements. This paper investigates joint reconstruction of received signal strength (RSS) maps and obstacle occupancy grids from sparse RSS samples. We formulate the task as conditional generation of a two-channel radio-environment map. A conditional flow matching (FM) framework transports Gaussian noise to the conditional map distribution, guided by sparse RSS observations and sensing masks. The method jointly reconstructs RSS fields and obstacle occupancy through one-step or few-step ordinary differential equation (ODE) sampling. Ray-tracing experiments show lower MSE than DDPM, DDNM, and RadioUNet across the tested sensing ratios. At a sensing ratio of 0.01, RSS and occupancy-grid MSE decrease by 57.8% and 62.4%, respectively, relative to DDPM. These results support FM as an efficient reconstruction component for THz ISAC.

**Index terms:** Terahertz ISAC, radio map, conditional flow matching, occupancy inference, blockage awareness.

---

## Method at a glance

| Component | Description |
| --- | --- |
| Joint map | `X = [P; S] ∈ R^{2×L×W}` — RSS field `P` and obstacle occupancy grid `S` reconstructed in one two-channel output |
| Conditioning | `C = [P̃; M] ∈ R^{2×L×W}` — sparse RSS observations `P̃ = M ⊙ P + Ξ` and binary sensing mask `M` |
| Transport | Linear path `X_t = tX_1 + (1−t)X_0`, target velocity `U_t = X_1 − X_0`; conditional U-Net `v_θ(X_t, t, C)` trained with `L_FM = E‖v_θ − U_t‖²` |
| Sampling | Euler integration of `dX_t/dt = v_θ(X_t, t, C)`, **1 step** in the reported experiments (vs. 1000 for DDPM, 500 for DDNM) |
| Observation consistency | `P̂ ← (1−M) ⊙ P̂ + M ⊙ P̃` — exactly preserves available RSS samples |

---

## Experimental setup

- **Channel data:** Remcom Wireless InSite ray tracing at **300 GHz**, 1 GHz bandwidth, `32 × 32` uniform planar array at a central BS.
- **Scenarios:** `100 m × 100 m` outdoor area, 1–6 randomly generated quadrilateral obstacles; RSS maps and occupancy grids rendered at `64 × 64`.
- **Sensing ratios:** `ρ ∈ {0.01, 0.05, 0.1, 0.2}`; noiseless observations (`Ξ = 0`).
- **Baselines:** DDPM (1000 sampling steps), DDNM (500 steps), RadioUNet (deterministic); comparable U-Net backbones, same data split and optimization settings.
- **Metrics:** MSE, SSIM, PSNR, reported separately for RSS and occupancy.

### Reconstruction MSE (lower is better; best per setting in bold)

| Method | ρ = 0.01 (RSS / Grid) | ρ = 0.05 (RSS / Grid) | ρ = 0.10 (RSS / Grid) | ρ = 0.20 (RSS / Grid) |
| --- | --- | --- | --- | --- |
| DDPM | 0.1031 / 0.1969 | 0.0565 / 0.1074 | 0.0420 / 0.0867 | 0.0235 / 0.0576 |
| DDNM | 0.1327 / 0.2123 | 0.0619 / 0.1070 | 0.0543 / 0.1081 | 0.0389 / 0.0667 |
| RadioUNet | 0.0508 / 0.0863 | 0.0275 / 0.0499 | 0.0194 / 0.0361 | 0.0139 / 0.0278 |
| **FM (ours)** | **0.0435 / 0.0741** | **0.0259 / 0.0478** | **0.0187 / 0.0346** | **0.0125 / 0.0274** |

### Headline results

- At `ρ = 0.01`: RSS MSE decreases from `0.1031` (DDPM) to `0.0435` — a **57.8%** reduction; occupancy-grid MSE decreases from `0.1969` to `0.0741` — a **62.4%** reduction.
- Versus the next-best method RadioUNet: RSS and occupancy-grid MSE decrease by **14.4%** and **14.1%**, respectively.
- FM gives the highest RSS and occupancy **PSNR at all tested sensing ratios with a single Euler step**.
- The advantage is consistent for MSE and PSNR but not universal for SSIM: DDPM has higher RSS SSIM at `ρ = 0.05, 0.1, 0.2`, while FM has the highest occupancy SSIM there (RadioUNet leads at `ρ = 0.01`).
- The reported step counts are sampler configurations, not measured wall-clock speedups; downstream beam-selection / resource-allocation performance is **not** evaluated in this paper.

---

## Award

**Best Student Paper Award**, 2026 IEEE 26th International Conference on Communication Technology (ICCT), Zhuhai, China, October 16–18, 2026.

---

## Citation

If you find this work useful, please cite:

```bibtex
@inproceedings{li2026jointradioenvironment,
  title     = {Joint Radio-Environment Map Construction for {THz} {ISAC} via Conditional Flow Matching},
  author    = {Li, Ruixuan and Wang, Shuai and Li, Lingxiang and Chen, Zhi},
  booktitle = {2026 IEEE 26th International Conference on Communication Technology (ICCT)},
  address   = {Zhuhai, China},
  year      = {2026},
  month     = oct,
  note      = {Best Student Paper Award}
}
```

A machine-readable record is provided in [`CITATION.cff`](CITATION.cff). DOI and page numbers will be added once the paper appears in IEEE Xplore.

---

## Acknowledgment

This work was supported in part by the Mobile Information Networks–National Science and Technology Major Project under Grant No. 2025ZD1305200.

---

## Copyright

This repository hosts the **author's manuscript** of the paper (the PDF has no IEEE copyright page). The paper is © 2026 IEEE. The author-posted version is provided here for personal and academic reading, as permitted by IEEE's author-rights policy; the citable version of record will be available through IEEE Xplore. Please do not redistribute the PDF commercially.

---

## 中文简介

本文研究太赫兹（THz）通信感知一体化（ISAC）中的**联合无线电环境地图构建**：在仅有极稀疏 RSS（接收信号强度）采样的条件下，同时重建稠密 RSS 场与障碍物占据栅格。我们把该任务建模为双通道无线电环境地图的条件生成问题，用**条件流匹配（conditional flow matching, FM）**学习一个速度场，将高斯噪声沿 ODE 输运到条件地图分布，再由观测一致性校正精确保留已测 RSS。基于 Wireless InSite 射线追踪的 THz 数据集实验表明：在 `ρ = 0.01` 的极稀疏采样下，相对 DDPM，RSS 与占据栅格 MSE 分别下降 **57.8%** 与 **62.4%**；相对次优的 RadioUNet 分别下降 **14.4%** 与 **14.1%**；且仅需 **1 步 Euler 采样**（DDPM 为 1000 步、DDNM 为 500 步），RSS 与占据栅格 PSNR 在所有测试采样率下均为最高。

论文获 **2026 IEEE 第 26 届国际通信技术大会（ICCT 2026，珠海，10 月 16–18 日）最佳学生论文奖（Best Student Paper Award）**。
