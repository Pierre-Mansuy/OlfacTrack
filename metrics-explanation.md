# Olfactrack Metrics Guide

This document explains the metrics calculated by Olfactrack to analyze insect behavior in Y-tube olfactometer experiments.

## Left/Right vs. Odor/Control

Olfactrack calculates metrics in two different reference frames:

### Left/Right (Spatial Coordinates)
- These metrics refer to the physical layout of the Y-tube
- Useful for detecting positional bias in your experimental setup
- Independent of experimental treatment
- Labeled with suffixes like `_left` and `_right`

### Odor/Control (Biological Significance)
- These metrics connect spatial behavior to experimental treatment
- Requires `Odor_Side` information in your metadata file
- Indicates actual olfactory preferences
- Labeled with suffixes like `_odor` and `_control`

The conversion from Left/Right to Odor/Control is performed automatically based on which arm contains the odor stimulus in each experiment.

## Types of Metrics

### Proportion Metrics

Proportion metrics range from 0 to 1 (or 0% to 100%) and indicate relative preference.

| Metric | Description | Interpretation |
|--------|-------------|----------------|
| `prop_left` / `prop_right` | Proportion of time spent in left/right arm | Values >0.5 indicate preference for that side |
| `prop_odor` / `prop_control` | Proportion of time spent in odor/control arm | `prop_odor` >0.5 indicates attraction to odor |
| `prop_bout_left` / `prop_bout_right` | Proportion of bouts (visits) to left/right arm tip | Measures frequency of visits rather than duration |
| `prop_bout_odor` / `prop_bout_control` | Proportion of bouts to odor/control arm | Measures preference based on visit frequency |
| `prop_proportion_left` / `prop_proportion_right` | Proportion based on summed time x position values for left/right | Time weighted by position within the arm (stronger at tips) |
| `prop_proportion_odor` / `prop_proportion_control` | Proportion based on summed time x position values for odor/control | Time weighted by position within the arm (stronger at tips |

### Time Metrics

These metrics measure actual time spent in different regions.

| Metric | Description | Units |
|--------|-------------|-------|
| `time_left` / `time_right` | Number of frames spent in left/right arm | Frames |
| `time_odor` / `time_control` | Number of frames spent in odor/control arm | Frames |
| `time_above_threshold_left` / `time_above_threshold_right` | Frames spent deeply in the arm (proportion > 0.75) | Frames |
| `time_odor_above_threshold` / `time_control_above_threshold` | Frames spent deeply in odor/control arm | Frames |

To convert frames to seconds, divide by your video's frame rate (default is 10fps).

### Positional Metrics

These metrics relate to the bumblebee's position in the Y-tube.

| Metric | Description | Range |
|--------|-------------|-------|
| `sum_proportion_left` / `sum_proportion_right` | Sum of proportion values in left/right arm | Higher = more time spent deep in arm |
| `sum_proportion_odor` / `sum_proportion_control` | Sum of proportion values in odor/control arm | Higher = more time spent deep in odor arm |
| `avg_proportion_left` / `avg_proportion_right` | Average depth in left/right arm | 0-1 (1 = tip of arm) |
| `avg_proportion_odor` / `avg_proportion_control` | Average depth in odor/control arm | 0-1 (1 = tip of arm) |

### Movement Metrics

These metrics quantify the bumblebee's movement patterns.

| Metric | Description | Units |
|--------|-------------|-------|
| `average_velocity_in_branch` | Average velocity across all branches | Pixels/frame |
| `overall_average_velocity` | Average velocity throughout experiment | Pixels/frame |
| `velocity_left` / `velocity_right` | Average velocity in left/right arm | Pixels/frame |
| `average_velocity_odor` / `average_velocity_control` | Average velocity in odor/control arm | Pixels/frame |
| `cumulative_distance` | Total distance traveled | Pixels |
| `cumulative_distance_left` / `cumulative_distance_right` | Distance traveled in left/right arm | Pixels |
| `cumulative_distance_odor` / `cumulative_distance_control` | Distance traveled in odor/control arm | Pixels |

### Decision Metrics

These metrics focus on choice behavior and transitions.

| Metric | Description | Values |
|--------|-------------|--------|
| `first_choice_lr` | First arm chosen (left/right) | "Left" or "Right" |
| `last_choice_lr` | Last arm visited (left/right) | "Left" or "Right" |
| `first_choice` | First arm chosen (odor/control) | "Odor" or "Control" |
| `last_choice` | Last arm visited (odor/control) | "Odor" or "Control" |
| `change_count` | Number of transitions between arms | Integer |
| `bout_left` / `bout_right` | Number of separate visits to left/right arm | Integer |
| `bout_odor` / `bout_control` | Number of separate visits to odor/control arm | Integer |

## Thresholds and Filtering

Olfactrack uses several thresholds to improve data quality and sensitivity:

### Proportion Thresholds
- **0.25 threshold**: Used to define "no choice" zone (central area)
- **0.75 threshold**: Used to define deep arm zones (tips of Y-tube)
- These allow metrics like `time_above_threshold` which count only when the bumblebee is fully committed to an arm

### Trajectory Filtering
- **Bottom segment filter**: Videos with excessive time in bottom segment are filtered out
- **Duration filter**: Videos outside the specified duration range are excluded
- **Tracking quality**: Isolated detections are cleaned using window-based filtering

## Interpreting Results

### Individual Trial Analysis
- **Strong attraction**: `prop_odor` > 0.7
- **Moderate attraction**: 0.6 < `prop_odor` < 0.7
- **Slight attraction**: 0.5 < `prop_odor` < 0.6
- **Neutral**: `prop_odor` ≈ 0.5
- **Repellency**: `prop_odor` < 0.5

### Statistical Analysis
For proper experimental analysis, consider:
- Calculating mean and standard error across multiple trials
- Using bootstrapping or appropriate statistical tests
- Comparing different treatments using metrics like `prop_odor`
- Examining both time-based metrics (`prop_odor`) and visit-based metrics (`prop_bout_odor`)

## Example Metrics Comparison

| Scenario | prop_left | prop_right | Cote_Odor | prop_odor | Interpretation |
|----------|-----------|------------|-----------|-----------|----------------|
| Attraction, odor on left | 0.75 | 0.25 | Left | 0.75 | Strong attraction |
| Attraction, odor on right | 0.25 | 0.75 | Right | 0.75 | Strong attraction |
| Repellent, odor on left | 0.25 | 0.75 | Left | 0.25 | Strong repellency |
| Repellent, odor on right | 0.75 | 0.25 | Right | 0.25 | Strong repellency |
| Positional bias | 0.70 | 0.30 | varies | ≈0.50 | No olfactory preference, but spatial bias |

## Summary Calculations

The final results.csv file contains individual trial metrics. The summary_statistics.csv file provides:
- Mean values for each metric grouped by experimental condition
- Standard error of the mean (SEM)
- Sample size (n)
- Other statistical measures (median, standard deviation)

Use these summary statistics for graphing and reporting experimental results.
