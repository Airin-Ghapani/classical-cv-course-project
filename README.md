# Classical Computer Vision — Four Challenges and a Capstone

A portfolio version of a university computer-vision course project built around **classical, interpretable methods**. The notebook covers boundary detection, single-image super-resolution, object counting, color constancy, and a JPEG-like compression capstone, with quantitative evaluation, ablations, and small learned comparisons.

> **Provenance:** the course scaffold supplied data-loading utilities, evaluation metrics, baseline helpers, synthetic-data generators, and evaluation cells. My contributions are the classical methods, ablations, experiments, learned comparison models, capstone implementation, result analysis, and reflections. The notebook labels scaffold and implementation sections explicitly.

## What I implemented

| Area | Classical method | Main comparison |
|---|---|---|
| Boundary detection | Multi-scale luminance + opponent-color gradients with local orientation coherence | Sobel + ridge-fusion comparison |
| Super-resolution | Edge-adaptive bicubic/Lanczos fusion with mild anti-ringing smoothing | Bicubic + learned patch-residual comparison |
| Counting | Multi-scale LoG seeds with image-adaptive single-cell area correction | Otsu connected components + ridge count calibrator |
| Color constancy | Robust fourth-order Gray-Edge with dark/highlight rejection | Gray-world, standard Gray-Edge + ridge illuminant regressor |
| Compression capstone | Activity-adaptive 8×8 DCT quantization with rate accounting | Uniform DCT, Pillow JPEG, and KLT/PCA transform |

## Key results

These are **small educational experiments**, not full-dataset or state-of-the-art benchmark claims.

| Experiment | Baseline | Final classical method | Small learned comparison |
|---|---:|---:|---:|
| Boundary detection — mean ODS F1 | Sobel: **0.449** | **0.542** | **0.578** |
| Super-resolution ×2 — mean PSNR / SSIM | Bicubic: **23.648 dB / 0.7163** | **23.652 dB / 0.7177** | ~**24.45 dB / 0.758** |
| Counting — mean absolute count error | Otsu+CC: **42.25** | **1.75** | **3.00** |
| Counting — mean Dice | — | **0.862** | — |
| Color constancy — mean / median angular error | Gray-world: **8.44° / 8.61°** | **4.04° / 4.12°** | **7.90° / 6.77°** |

### Capstone result

At three approximately matched-rate operating points, activity-adaptive DCT quantization improved **edge-region PSNR** by **+0.151, +0.122, and +0.103 dB**, while global PSNR changed very little and SSIM decreased. The conclusion is intentionally mixed: the hand-designed activity rule improves the contour-focused metric slightly, but does **not** provide a general rate-distortion win.

A held-out KLT/PCA transform trained on three images and tested on the fourth produced PSNR gains of **+2.16, +1.52, and +0.96 dB** over the nearest-rate fixed-DCT points.

## Repository structure

```text
classical-cv-course-project/
├── README.md
├── NOTICE.md
├── requirements.txt
├── .gitignore
├── notebooks/
│   └── classical_cv_course_project.ipynb
├── scripts/
│   └── download_bsds_samples.py
└── data/
    └── README.md
```

The original course notebook is preserved unchanged, including its saved
outputs. Raw BSDS500 dataset files are not distributed separately in this
repository.

## Setup

Python 3.10+ is recommended.

```bash
python -m venv .venv
```

Activate the environment, then install the dependencies:

```bash
pip install -r requirements.txt
```

### Optional: reproduce the four BSDS sample experiments

The repository does not redistribute BSDS500 images or ground-truth files. To download the official Berkeley archive and extract only the four sample IDs used by this project:

```bash
python scripts/download_bsds_samples.py
```

This creates:

```text
data/bsds/images/
data/bsds/gt/
```

The selected sample IDs are:

```text
100007
100039
101027
102062
```

Without these files, supported parts of the notebook fall back to generated synthetic data.

## Run

Start Jupyter:

```bash
jupyter lab
```

Open:

```text
notebooks/classical_cv_course_project.ipynb
```

Then use **Restart Kernel and Run All Cells**.

The notebook uses:

```python
SEED = 7
```

for deterministic random experiments where applicable.

## Reproducibility notes

- The saved text outputs come from the original course-project run using four local BSDS500 samples.
- The original notebook is preserved in its course-project form, including saved outputs from the original experiments.
- The counting benchmark is synthetic and uses known ground-truth counts/masks.
- Learned comparisons are deliberately lightweight and implemented without pretrained external checkpoints.
- The reported results are intended to demonstrate methodology, ablation design, and analysis rather than claim benchmark leadership.

## Limitations

- Boundary, super-resolution, color-constancy, and capstone experiments use a very small natural-image sample.
- The boundary evaluation is ODS-style but is not a replacement for the full official BSDS500 benchmark protocol.
- The super-resolution improvement from the classical method is small.
- The counting experiment uses synthetic fluorescence-style imagery rather than a full real-cell benchmark.
- The compression capstone is luminance-only and does not implement full JPEG syntax, chroma subsampling, deblocking, or optimized entropy coding.
- The KLT comparison does not charge model/basis storage in its rate estimate.

## References

- P. Arbelaez, M. Maire, C. Fowlkes, J. Malik, **Contour Detection and Hierarchical Image Segmentation**, IEEE TPAMI, 2011.
- X. Xie, Z. Tu, **Holistically-Nested Edge Detection**, ICCV, 2015.
- P. Dollár, C. Zitnick, **Fast Edge Detection Using Structured Forests**, IEEE TPAMI, 2015.
- C. Dong et al., **Learning a Deep Convolutional Network for Image Super-Resolution (SRCNN)**, ECCV, 2014.
- B. Lim et al., **Enhanced Deep Residual Networks for Single Image Super-Resolution (EDSR)**, CVPR Workshops, 2017.
- C. Stringer et al., **Cellpose: a generalist algorithm for cellular segmentation**, Nature Methods, 2021.
- U. Schmidt et al., **Cell Detection with Star-Convex Polygons (StarDist)**, MICCAI, 2018.
- J. van de Weijer et al., **Edge-Based Color Constancy**, IEEE TIP, 2007.
- J. T. Barron, **Convolutional Color Constancy**, ICCV, 2015.
- G. K. Wallace, **The JPEG Still Picture Compression Standard**, 1992.
- J. Ballé et al., **End-to-End Optimized Image Compression**, ICLR, 2017.

For BSDS500 downloads and benchmark guidance, use the official Berkeley Computer Vision Group resource page:
https://www2.eecs.berkeley.edu/Research/Projects/CS/vision/grouping/resources.html
