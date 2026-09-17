# DIP Mini Project — Restoration of Fog/Haze-Degraded Outdoor Images

**Course:** 265DSEUBC302B – Digital Image Processing
**Mapped Course Outcomes:** CO4 (Restoration & Enhancement), CO5 (Analysis & Interpretation of Results)

## Overview

This project restores outdoor photographs degraded by fog/haze using the
**Dark Channel Prior (DCP)** algorithm combined with a **manually
implemented guided filter** for transmission-map refinement. Because a
real hazy/clean pair of the same scene is hard to obtain, a synthetic
hazy image is generated from a clean photo using the atmospheric
scattering model, which also gives a ground-truth reference for
objective evaluation.

## Pipeline

```
Clean Input Image
   -> Synthetic Haze Generation (Atmospheric Scattering Model)
   -> Dark Channel Computation
   -> Atmospheric Light Estimation
   -> Raw Transmission Map Estimation
   -> Transmission Map Refinement (manual Guided Filter)
   -> Scene Radiance Recovery (Dehazing)
   -> Result Comparison
   -> Quantitative Evaluation (PSNR, SSIM)
```

## Repository Structure

```
DIP-Project/
├── README.md            <- this file
├── source_code.py        <- full, commented implementation
├── images/                <- input image(s) used
├── screenshots/            <- execution screenshots / output figure
├── report/                 <- project report (.docx)
└── requirements.txt        <- Python dependencies
```

## How to Run

1. Install dependencies:
   ```
   pip install -r requirements.txt
   ```
2. Place your clean input photo at `images/photo1.jfif` (or update
   `INPUT_IMAGE_PATH` in `source_code.py`).
3. Run:
   ```
   python source_code.py
   ```
4. Outputs (`CLEAN.jpg`, `HAZY.jpg`, `DEHAZED.jpg`,
   `output_comparison.png`) are written to the working directory, and
   PSNR/SSIM metrics are printed to the console.

## Results

| Metric    | Hazy vs Clean (Before) | Dehazed vs Clean (After) |
|-----------|:-----------------------:|:--------------------------:|
| PSNR (dB) | 12.71                   | 17.76                      |
| SSIM      | 0.766                   | 0.924                      |

The dehazed output shows a visibly clearer sky, restored contrast on
building surfaces, and colour closer to the original clean image.

## Key Parameters

| Parameter                     | Value      |
|--------------------------------|-----------|
| Dark-channel patch size        | 15 × 15   |
| Haze retention (omega)         | 0.95      |
| Guided filter radius           | 40        |
| Guided filter epsilon          | 0.001     |
| Minimum transmission (t0)      | 0.1       |
| Synthetic atmospheric light (A)| [0.9, 0.9, 0.9] |
| Synthetic transmission (t)     | 0.55      |

## Limitations

- Evaluated on a single image with synthetically generated, spatially
  uniform haze — not yet tested on real, non-uniform fog.
- Assumes a single flat atmospheric light value across the image.
- Dark Channel Prior can produce halo artefacts in large sky regions.
- Parameters are fixed manually rather than tuned adaptively.

## Future Scope

- Evaluate on real hazy-image benchmarks (RESIDE, O-HAZE, I-HAZE).
- Compare against learning-based dehazing (AOD-Net, FFA-Net).
- Adaptive parameter selection and sky-aware segmentation.
- Real-time / mobile deployment.

See `report/` for the full project report with detailed methodology,
dataset description, evaluation and discussion.
