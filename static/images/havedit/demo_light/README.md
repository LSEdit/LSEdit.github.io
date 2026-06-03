# Demo Light Web Assets

This directory contains the lightweight WebP assets for the webpage demo.

It is derived from `web_assets/demo_output`, but only keeps the images needed for the interactive webpage:

- original image
- final result image for each parameter setting
- semantic-prior-current overlay on the original image for each time step

## Directory Structure

```text
demo_light/
  README.md
  original.webp
  sweep_manifest.json

  grid/
    threshold_0p8__bhc_tau_low_0p6__bhc_lambda_max_0/
      manifest.json
      result.webp
      frames/
        overlay_semantic_prior/
          step_0001.webp
          step_0002.webp
          ...
          step_0028.webp

    threshold_0p8__bhc_tau_low_0p6__bhc_lambda_max_0p03/
      ...
```

## What To Display

Use `original.webp` as the fixed input image.

For each parameter combination, use:

- `result.webp`: final edited result for that parameter setting
- `frames/overlay_semantic_prior/step_XXXX.webp`: semantic-prior-current heatmap at time step `XXXX`, blended on the original image

The overlay images are not blended on `x0^t`; they are blended on the original image.

## Parameter Grid

The webpage has three parameter sliders plus one time-step slider.

```text
threshold:
  0.80, 0.82, 0.84, 0.86, 0.88, 0.90, 0.92, 0.94, 0.96

bhc_tau_low:
  0.60, 0.65, 0.70, 0.75, 0.80, 0.85, 0.88

bhc_lambda_max:
  0.00, 0.03, 0.06, 0.09, 0.12, 0.15, 0.18, 0.21, 0.24, 0.27, 0.30

time step:
  1, 2, ..., 28
```

Total variants:

```text
9 * 7 * 11 = 693
```

Each variant has 28 overlay frames.

## Manifest Loading

Start from:

```text
sweep_manifest.json
```

Important fields:

```text
assets.original
  Top-level original image path: original.webp

sliders.threshold
sliders.bhc_tau_low
sliders.bhc_lambda_max
sliders.time_step
  Slider values for the webpage.

variants[]
  List of all parameter combinations.

variants[].output_dir
  Directory for this parameter setting.

variants[].manifest
  Per-variant manifest path.

variants[].result
  Final edited result path for this parameter setting.
```

Each per-variant `manifest.json` contains:

```text
assets.original
  Relative path back to ../../original.webp

assets.result
  result.webp

frames[].step
  Time step index.

frames[].t_norm
  Normalized flow-matching time value.

frames[].overlay_semantic_prior
  WebP overlay path for this step.
```

## Path Example

For this parameter setting:

```text
threshold = 0.80
bhc_tau_low = 0.60
bhc_lambda_max = 0.00
```

Variant directory:

```text
grid/threshold_0p8__bhc_tau_low_0p6__bhc_lambda_max_0/
```

Final result:

```text
grid/threshold_0p8__bhc_tau_low_0p6__bhc_lambda_max_0/result.webp
```

Overlay at time step 7:

```text
grid/threshold_0p8__bhc_tau_low_0p6__bhc_lambda_max_0/frames/overlay_semantic_prior/step_0007.webp
```

## Not Included

The following heavy/raw assets are intentionally not included:

- `frames/x0/`
- `frames/semantic_prior/`
- `frames/attention/`
- `metrics.json`
- `metrics.csv`
- per-variant `original.png`
- per-variant `result.png`
- per-variant `target_mask.png`
- `edit_mask`, `preserve_weight`, and their overlays

## Asset Summary

```text
variants: 693
frames per variant: 28
overlay frames: 693 * 28 = 19404
result images: 693
top-level original image: 1
format: WebP
quality: 82
overlay alpha: 0.42
```
