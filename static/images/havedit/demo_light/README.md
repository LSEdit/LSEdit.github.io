# Demo Light Web Assets

This directory is a lightweight WebP subset derived from `web_assets/demo_output`.

Kept:
- `original.webp`
- `grid/*/frames/x0/*.webp`
- `grid/*/frames/overlay_semantic_prior/*.webp`
- lightweight `sweep_manifest.json` and per-variant `manifest.json`

Not copied:
- `frames/semantic_prior/`
- `frames/attention/`
- `metrics.json`
- `metrics.csv`
- per-variant `original.png`
- per-variant `result.png`
- per-variant `target_mask.png`
- `edit_mask`, `preserve_weight`, and their overlays

Current parameter grid:

```text
threshold:
  0.80, 0.82, 0.84, 0.86, 0.88, 0.90, 0.92, 0.94, 0.96

bhc_tau_low:
  0.60, 0.70, 0.80, 0.85, 0.88

bhc_lambda_max:
  0.00, 0.03, 0.06, 0.09, 0.12, 0.15, 0.18, 0.21, 0.24
```

Intended webpage controls:
- parameter sliders: `threshold`, `bhc_tau_low`, `bhc_lambda_max`
- time slider: `x0^t` frame step
- overlay toggle: `None` / `Semantic Prior Current`

Variants: `405`
Frames per variant: `28`
