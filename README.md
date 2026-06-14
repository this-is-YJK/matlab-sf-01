# MATLAB Tracking Scenario Simulator

A modular, extensible platform for simulating multi-platform tracking scenarios with geodetic and ECEF coordinate support.

## Overview

This simulator generates realistic platform trajectories for tracking and sensor fusion research. It supports multiple ownships and targets with configurable waypoints in either geodetic (LLA) or ECEF (XYZ) coordinates.

## Requirements

- MATLAB R2020a or later
- Sensor Fusion and Tracking Toolbox (for `trackingScenario`)
- Mapping Toolbox (for WGS84 ellipsoid functions)

## Quick Start

```matlab
% Run with default configuration
run senario_10062026_01.m

% Or run with custom configuration
config = scenario_config();
% ... modify config ...
run_scenario(config);
```

## File Structure

```
src/
  senario_10062026_01.m     - Main entry point
  scenario_config.m          - Configuration definition
  run_scenario.m             - Simulation orchestrator
  create_platforms.m         - Platform creation
  waypoint_utils.m           - Coordinate conversions (LLA/ECEF)
  attitude_utils.m           - Attitude calculations (heading, pitch, roll)
  data_recorder.m            - Data capture and file output
  plot_trajectories.m        - 3D visualization
  example_custom_scenario.m  - Usage examples
```

## Configuration

### Adding Platforms

```matlab
% Get base configuration
config = scenario_config();

% Extend with new platform
n = length(config.platforms);
config.platforms(n+1) = config.platforms(1);  % Copy template
config.platforms(n+1).id = 3;
config.platforms(n+1).name = "T_3";
config.platforms(n+1).type = 'target';
config.platforms(n+1).coordSystem = 'lla';  % or 'ecef'
config.platforms(n+1).waypoints = [lat, lon, alt];
config.platforms(n+1).times = [t0; tf];

run_scenario(config);
```

### Coordinate Systems

| System | Units | Format |
|--------|-------|--------|
| `lla` | degrees, degrees, meters | [latitude, longitude, altitude] |
| `ecef` | meters | [X, Y, Z] Earth-Centered Earth-Fixed |

### Platform Types

- `ownship` - Blue force platforms (plotted in blue)
- `target` - Red force platforms (plotted in red)

## Output Format

The simulator generates a CSV file with the following columns:

| Column | Description | Units |
|--------|-------------|-------|
| Sample No | Sequential sample number | - |
| Time | Simulation time | seconds |
| target_ID | Platform identifier | integer |
| Target_name | Platform name | string |
| Latitude | Geodetic latitude | degrees |
| Longitude | Geodetic longitude | degrees |
| Altitude | Height above WGS84 ellipsoid | meters |
| Velocity_x | ECEF X velocity component | m/s |
| Velocity_y | ECEF Y velocity component | m/s |
| Velocity_z | ECEF Z velocity component | m/s |
| Heading | Azimuth angle | degrees |
| Roll | Bank angle | degrees |
| Pitch | Elevation angle | degrees |
| Yaw | Same as heading | degrees |

## Mathematics Reference

See `docs/mathematics.html` for detailed mathematical derivations of:

- Coordinate transformations (ECEF/LLA)
- Attitude calculation algorithms
- Waypoint interpolation
- Trajectory generation

## Customization

### Modifying Update Rate and Duration

```matlab
config.scenario.updateRate = 10;   % 10 Hz
config.scenario.stopTime = 500;    % 500 seconds
```

### Custom Output Filename

```matlab
config.output.filename = 'my_scenario.scenario';
config.output.enabled = true;
```

### Adding Complex Trajectories

```matlab
% Multi-leg flight path with intermediate waypoints
config.platforms(1).waypoints = [
    35.0  45.0  10000;  % Start
    35.0  46.0  12000;  % Climb leg
    34.5  47.0  12000;  % Level turn
    34.0  48.0  10000]; % Descent
config.platforms(1).times = [0; 200; 400; 600];
```

## License

MIT License - Free for academic and research use.
