# Olfactrack

## 2D Tracking Pipeline for Insect Behavior in Olfactometer Videos

Olfactrack is a Python-based tracking pipeline designed for analyzing insect behavior in Y-tube olfactometer experiments. The system enables precise tracking of insect movement through the Y-tube, calculation of behavioral metrics, and generation of visualizations to understand olfactory preferences.

## Features

- **Precise ROI Selection**: Manually define the Y-tube structure with simple point-and-click interface
- **Computer Vision Tracking**: Track insect movement using background subtraction and contour detection
- **Comprehensive Analysis**: Calculate behavioral metrics such as time spent in each arm, preference indices, and movement patterns
- **Rich Visualizations**: Generate heatmaps, trajectory plots, and animations to visualize behavior
- **Parallel Processing**: Process multiple videos simultaneously for faster analysis
- **Experimental Integration**: Merge tracking data with experimental metadata for statistical analysis

## Installation

### Prerequisites

- Python 3.7 or higher
- Jupyter Notebook or JupyterLab

### Required Libraries

```bash
pip install opencv-python numpy pandas matplotlib seaborn jupyter
```

### Getting Started

1. Clone this repository:
   ```bash
   git clone https://github.com/yourusername/olfactrack.git
   cd olfactrack
   ```

2. Launch Jupyter Notebook:
   ```bash
   jupyter notebook
   ```

3. Open the `olfactrack.ipynb` notebook

### Video Preparation

For optimal tracking performance, videos should be prepared as follows:

Cropping: Videos should be cropped to focus only on the Y-tube area (recommended)
Resolution: 720p resolution is often sufficient
Frame Rate: 10 fps is adequate and helps reduce file size and processing time

See example files in the repository for reference. Preprocessing your videos according to these specifications will improve tracking efficiency and reduce computational requirements.

## Usage

The tracking pipeline is implemented as a Jupyter Notebook with five sequential stages:

1. **Import & Setup**: Configure libraries and environment
2. **ROI Selection**: Define Y-tube structure by selecting key points
3. **Tracking**: Process videos to track insect movement
4. **Visualization**: Generate plots and animations of trajectories
5. **Analysis**: Calculate behavioral metrics and statistics

### Basic Workflow

1. Place your olfactometer videos in a dedicated folder
2. Configure the folder path in the notebook
3. Run the ROI selection to mark key points in each video
4. Execute the tracking section to process all videos
5. Generate visualizations to understand movement patterns
6. Run analysis to calculate metrics and produce statistics

## Configuration Parameters

Each section of the notebook contains a configuration block where parameters can be adjusted:

```python
# ===== TRACKING CONFIGURATION =====
# Background subtraction parameters
HISTORY = 500                      # History length for background subtractor
VAR_THRESHOLD = 200                # Threshold for background subtractor
# ... more parameters ...
# ===================================
```

Adjust these parameters based on your specific experimental setup and video characteristics.

## Output Files

The pipeline produces several output files:

- **selected_points.csv**: Contains ROI coordinates for each video
- **processed_{video_name}.csv**: Tracking data with X,Y coordinates and metrics
- **results.csv**: Summary file with tracking metrics for all videos
- **summary_statistics.csv**: Statistical summaries grouped by experimental conditions
- **Visualization images**: PNG files for trajectory plots, heatmaps, etc.
- **Animations**: GIF files showing insect movement (optional)

## Example Visualizations

### Raw Trajectory Plot
![Raw Trajectory](https://github.com/Pierre-Mansuy/Olfactrack/blob/examples/102.mp4_raw_trajectory.png)

### Raw Trajectory Animation
![Raw Trajectory](https://github.com/Pierre-Mansuy/Olfactrack/blob/examples/102.mp4_animation.gif)

### Heatmap
![Heatmap](https://github.com/Pierre-Mansuy/Olfactrack/blob/examples/102.mp4_heatmap.png)

### Proportion Plot
![Proportion Plot](https://github.com/Pierre-Mansuy/Olfactrack/blob/examples/102.mp4_proportion.png)

## Key Metrics

The analysis calculates several behavioral metrics:

- **Time metrics**: Duration spent in each Y-tube arm
- **Velocity metrics**: Movement speed in different regions
- **Decision metrics**: First and last choices, change frequency
- **Bout analysis**: Number of times the insect reaches the tip of an arm.

Read the key metric file for full explanations. 

## Metadata Integration

For complete analysis, create a metadata CSV file with experimental details such as:

```
ID,Treatment,Odor_Side,etc
Insect_1,Treated,Left,...
Insect_2,Control,Right,...
...
```

The `Odor_Side` column is essential for converting left/right metrics to odor/control metrics. (must be "Left" or "Right")

## Example Outputs

![Boxplot](https://github.com/Pierre-Mansuy/Olfactrack/blob/examples/key_metrics.png)

![Correlation Plot](https://github.com/Pierre-Mansuy/Olfactrack/blob/examples/correlation.png)

![Heatmap](https://github.com/Pierre-Mansuy/Olfactrack/blob/examples/heatmap.png)

## Customization

Olfactrack can be adapted to different Y-tube designs and experimental setups by adjusting:

- Y-tube thickness parameters
- Detection thresholds
- Filtering criteria
- Visualization options

## License

This project is licensed under the MIT License - see the LICENSE file for details.

## Citation

If you use this software in your research, please cite:

```
Mansuy, P. (2025). Olfactrack: 2D Tracking Pipeline for Insect Behavior in Olfactometer Videos. 
GitHub repository, https://github.com/Pierre-Mansuy/Olfactrack
```

## Contact

For questions, support, or collaboration:

- Email: bidje126jaag@gmail.com
- GitHub Issues: https://github.com/Pierre-Mansuy/Olfactrack/issues

---

*Developed for behavioral ecologists studying insect olfactory preferences*
