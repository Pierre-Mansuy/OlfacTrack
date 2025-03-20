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

### Time-Based Metrics

These metrics measure actual time spent in different regions.

| Metric | Description | Units |
|--------|-------------|-------|
| `time_left` / `time_right` | Number of frames spent in left/right arm | Frames |
| `time_odor` / `time_control` | Number of frames spent in odor/control arm | Frames |
| `prop_time_left` / `prop_time_right` | Proportion of time spent in left/right arm | 0-1 |
| `prop_time_odor` / `prop_time_control` | Proportion of time spent in odor/control arm | 0-1 |
| `time_above_threshold_left` / `time_above_threshold_right` | Frames spent deeply in the arm (arm_progression > 0.75) | Frames |
| `time_above_threshold_odor` / `time_above_threshold_control` | Frames spent deeply in odor/control arm | Frames |
| `prop_time_above_threshold_left` / `prop_time_above_threshold_right` | Proportion of threshold time in left/right arm | 0-1 |
| `prop_time_above_threshold_odor` / `prop_time_above_threshold_control` | Proportion of threshold time in odor/control arm | 0-1 |

To convert frames to seconds, divide by your video's frame rate (default is 10fps).

### Arm Progression Metrics

These metrics relate to how far the insect travels into each arm of the Y-tube.

| Metric | Description | Range |
|--------|-------------|-------|
| `weighted_arm_progression_left` / `weighted_arm_progression_right` | Sum of arm progression values in left/right arm (time-integrated position) | Higher = more time spent deep in arm |
| `weighted_arm_progression_odor` / `weighted_arm_progression_control` | Sum of arm progression values in odor/control arm | Higher = more time spent deep in odor arm |
| `avg_arm_progression_left` / `avg_arm_progression_right` | Average depth in left/right arm | 0-1 (1 = tip of arm) |
| `avg_arm_progression_odor` / `avg_arm_progression_control` | Average depth in odor/control arm | 0-1 (1 = tip of arm) |
| `prop_weighted_arm_progression_left` / `prop_weighted_arm_progression_right` | Proportion of weighted arm progression in left/right | 0-1 |
| `prop_weighted_arm_progression_odor` / `prop_weighted_arm_progression_control` | Proportion of weighted arm progression in odor/control | 0-1 |

### Movement Metrics

#### Velocity Metrics
| Metric | Description | Units |
|--------|-------------|-------|
| `average_velocity_in_branch` | Average velocity across all branches | Progression/frame |
| `overall_average_velocity` | Average velocity throughout experiment | Progression/frame |
| `velocity_left` / `velocity_right` | Average velocity in left/right arm | Progression/frame |
| `velocity_odor` / `velocity_control` | Average velocity in odor/control arm | Progression/frame |
| `prop_velocity_left` / `prop_velocity_right` | Proportion of velocity in left/right arm | 0-1 |
| `prop_velocity_odor` / `prop_velocity_control` | Proportion of velocity in odor/control arm | 0-1 |

#### Distance Metrics
| Metric | Description | Units |
|--------|-------------|-------|
| `cumulative_distance` | Total distance traveled | Progression units |
| `cumulative_distance_left` / `cumulative_distance_right` | Distance traveled in left/right arm | Progression units |
| `cumulative_distance_odor` / `cumulative_distance_control` | Distance traveled in odor/control arm | Progression units |
| `prop_cumulative_distance_left` / `prop_cumulative_distance_right` | Proportion of distance in left/right arm | 0-1 |
| `prop_cumulative_distance_odor` / `prop_cumulative_distance_control` | Proportion of distance in odor/control arm | 0-1 |

### Decision Metrics

#### Choice Metrics
| Metric | Description | Values |
|--------|-------------|--------|
| `first_choice_lr` | First arm chosen (left/right) | "Left" or "Right" |
| `last_choice_lr` | Last arm visited (left/right) | "Left" or "Right" |
| `first_choice` | First arm chosen (odor/control) | "Odor" or "Control" |
| `last_choice` | Last arm visited (odor/control) | "Odor" or "Control" |
| `change_count` | Number of transitions between arms | Integer |

#### Bout Metrics
| Metric | Description | Values |
|--------|-------------|--------|
| `bout_left` / `bout_right` | Number of separate visits to left/right arm tip | Integer |
| `bout_odor` / `bout_control` | Number of separate visits to odor/control arm tip | Integer |
| `prop_bout_left` / `prop_bout_right` | Proportion of bouts to left/right arm tip | 0-1 |
| `prop_bout_odor` / `prop_bout_control` | Proportion of bouts to odor/control arm tip | 0-1 |

#### Back-and-Forth Metrics
| Metric | Description | Values |
|--------|-------------|--------|
| `baf_left` / `baf_right` | Number of back-and-forth movements in left/right arm | Integer |
| `baf_odor` / `baf_control` | Number of back-and-forth movements in odor/control arm | Integer |
| `prop_baf_left` / `prop_baf_right` | Proportion of back-and-forth movements in left/right arm | 0-1 |
| `prop_baf_odor` / `prop_baf_control` | Proportion of back-and-forth movements in odor/control arm | 0-1 |

## Thresholds and Filtering

Olfactrack uses several thresholds to improve data quality and sensitivity:

### Arm Progression Thresholds
- **no_choice_threshold (0.25)**: Used to define "no choice" zone (central area)
- **tip_threshold (0.75)**: Used to define deep arm zones (tips of Y-tube)
- These allow metrics like `time_above_threshold` which count only when the insect is fully committed to an arm

### Trajectory Filtering
- **Bottom segment filter**: Videos with excessive time in bottom segment are filtered out
- **Duration filter**: Videos outside the specified duration range are excluded
- **Tracking quality**: Isolated detections are cleaned using window-based filtering

## Example Metrics Comparison

| Scenario | prop_time_left | prop_time_right | Odor_Side | prop_time_odor | Interpretation |
|----------|-----------|------------|-----------|----------------|----------------|
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
