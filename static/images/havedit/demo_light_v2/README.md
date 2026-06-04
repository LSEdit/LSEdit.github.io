# Demo Light v2 Web Assets

This directory contains lightweight WebP assets for the HAVEdit FLUX.2 webpage demo.

Sample:

```text
000000000025
```

Prompt:

```text
change the woman to a storm-trooper
```

This v2 export fixes an important parameter relation:

```text
threshold = bhc_tau_high
bhc_tau_low < threshold
```

Only valid parameter combinations satisfying that rule are included.

## Default Initial State

Use this parameter setting as the webpage's initial/default state:

```text
threshold = 0.90
bhc_tau_high = 0.90
bhc_tau_low = 0.60
bhc_lambda_max = 0.15
time step = 1
```

Equivalent CLI parameters:

```text
--threshold 0.90
--bhc-tau-low 0.60
--bhc-tau-high 0.90
--bhc-lambda-max 0.15
```

Default variant directory:

```text
grid/threshold_0p9__bhc_tau_low_0p6__bhc_lambda_max_0p15/
```

## Directory Structure

```text
demo_light_v2/
  README.md
  original.webp
  sweep_manifest.json

  grid/
    threshold_0p9__bhc_tau_low_0p6__bhc_lambda_max_0p15/
      manifest.json
      result.webp
      frames/
        overlay_semantic_prior/
          step_0001.webp
          step_0002.webp
          ...
          step_0028.webp
```

## Webpage Display

Use these assets in the webpage:

```text
original.webp
  Fixed input image.

grid/*/result.webp
  Final edited result for one parameter setting.

grid/*/frames/overlay_semantic_prior/step_XXXX.webp
  Semantic-prior-current heatmap at flow-matching step XXXX,
  blended on the original image.
```

Important: `overlay_semantic_prior` is blended on the original image, not on `x0^t`.

## Slider Values

The webpage has four sliders:

```text
threshold / bhc_tau_high:
  0.80, 0.82, 0.84, 0.86, 0.88, 0.90, 0.92, 0.94, 0.96

bhc_tau_low:
  0.60, 0.65, 0.70, 0.75, 0.80, 0.85, 0.88

bhc_lambda_max:
  0.00, 0.03, 0.06, 0.09, 0.12, 0.15, 0.18, 0.21, 0.24, 0.27, 0.30

time step:
  1, 2, ..., 28
```

Because `bhc_tau_low` must be lower than `threshold`, not every cartesian product exists.
Use `valid_tau_low_by_threshold` in `sweep_manifest.json` to dynamically restrict the `bhc_tau_low` slider.

Example:

```text
threshold = 0.80
valid bhc_tau_low values:
  0.60, 0.65, 0.70, 0.75
```

## Manifest Loading

Start from:

```text
sweep_manifest.json
```

Important top-level fields:

```text
assets.original
  Path to the fixed input image: original.webp

sliders.threshold
sliders.bhc_tau_high
sliders.bhc_tau_low
sliders.bhc_lambda_max
sliders.time_step
  Slider values.

valid_tau_low_by_threshold
  Map from threshold value to valid bhc_tau_low values.

variants[]
  All valid parameter combinations.

variants[].output_dir
  Directory for this parameter setting.

variants[].manifest
  Per-variant manifest path.

variants[].result
  Final result path.
```

Each per-variant `manifest.json` contains:

```text
assets.original
  Relative path back to ../../original.webp

assets.result
  result.webp

config.threshold
config.bhc_tau_high
config.bhc_tau_low
config.bhc_lambda_max
  Exact parameters for this variant.

frames[].step
  Time-step index.

frames[].t_norm
  Normalized flow-matching time.

frames[].overlay_semantic_prior
  WebP overlay path for this step.
```

## Path Example

For:

```text
threshold = bhc_tau_high = 0.90
bhc_tau_low = 0.60
bhc_lambda_max = 0.15
```

Variant directory:

```text
grid/threshold_0p9__bhc_tau_low_0p6__bhc_lambda_max_0p15/
```

Final result:

```text
grid/threshold_0p9__bhc_tau_low_0p6__bhc_lambda_max_0p15/result.webp
```

Overlay at time step 7:

```text
grid/threshold_0p9__bhc_tau_low_0p6__bhc_lambda_max_0p15/frames/overlay_semantic_prior/step_0007.webp
```

## Completion Check

This export is complete:

```text
complete: true
valid variants: 594
invalid skipped combinations: 99
status ok variants: 594
frames per variant: 28
overlay frames: 594 * 28 = 16632
result images: 594
```

## Not Included

The following heavy/raw assets are intentionally not included in the final web dataset:

```text
frames/x0/
frames/semantic_prior/
frames/attention/
metrics.json
metrics.csv
original.png
result.png
target_mask.png
edit_mask/
preserve_weight/
overlay_edit_mask/
overlay_preserve_weight/
```

## Asset Summary

```text
format: WebP
quality: 82
overlay alpha: 0.42
directory size after export: about 423M
```
