# Multi-Camera Capture System Documentation

## Overview

This system enables high-speed capture and reconstruction of dynamic objects using 32 synchronized Raspberry Pi Global Shutter cameras. The architecture prioritizes temporal resolution through hardware-based synchronization and efficient raw data capture, making it ideal for motion analysis, 3D reconstruction, and high-speed imaging applications.

## System Architecture
<div align="left">
  <img  width="700px" src="https://github.com/kennychufk/telefacet-doc/raw/main/img/system-architecture.png">
</div>

## System Components

### 1. Camera Array
- **32 Raspberry Pi Global Shutter Cameras** (IMX296 sensors)
- Connected in pairs to Raspberry Pi 5 computers via **CSI-2 interface**
- Capture raw SRGGB10P format (10-bit Bayer pattern)
- Resolution: Up to 1456×1088 pixels at high frame rates

### 2. Compute Nodes
- **16 Raspberry Pi 5 computers** (one per camera pair)
- Each runs the [cherupi-v4l2](https://github.com/kennychufk/cherupi-v4l2) camera control software
- Provides WebSocket server for remote control and frame streaming
- Handles camera discovery, configuration, and frame capture

### 3. Network Infrastructure
- **Router with Ethernet switches**
- Connects all 16 Raspberry Pi computers
- User access via WiFi or wired LAN connection
- Enables centralized control of all cameras

### 4. Synchronization System
- **ESP32-based PWM trigger generator** ([cherupi-pwm-trigger](https://github.com/kennychufk/cherupi-pwm-trigger))
- Generates precise rectangular wave pulses
- Distributes sync signals via **SMA coaxial cables** to camera XTR terminals
- Ensures frame-synchronized capture across all 32 cameras
- Configurable via web interface for timing adjustment

### 5. Control Interface
- **Web-based client application** ([telefacet-web](https://github.com/kennychufk/telefacet-web))
- Real-time preview of camera feeds
- Recording control and configuration
- Multi-camera coordination
- Accessible from any device with a web browser

### 6. Post-Processing Tools
- **Debayer utility** ([cherupi-debayer](https://github.com/kennychufk/cherupi-debayer))
- Converts raw SRGGB10P format to standard image formats
- Supports PNG and JPEG export
- Hardware-accelerated processing (SIMD optimization)

## Data Flow
<div align="left">
  <img  width="700px" src="https://github.com/kennychufk/telefacet-doc/raw/main/img/data-flow.png">
</div>

## Key Features

### Hardware Synchronization
- Sub-microsecond trigger accuracy using ESP32 hardware PWM
- Simultaneous frame capture across all 32 cameras
- Eliminates motion artifacts in multi-view reconstruction

### High-Throughput Capture
- Raw SRGGB10P format maximizes frame rate and data quality
- Minimal processing overhead during capture
- Direct memory-mapped buffer access for low latency

### Flexible Control
- Web-based interface accessible from any device
- Real-time preview with WebGL-accelerated debayering
- Per-camera configuration and streaming control
- Multiple recording modes (continuous, batch, checkerboard detection)

### Scalable Architecture
- Distributed processing across 16 compute nodes
- Network bandwidth shared efficiently
- Modular design allows easy expansion

## Typical Workflow

1. **Setup**: Power on all Raspberry Pi computers and ESP32 trigger generator
2. **Connect**: Join the network via WiFi or Ethernet cable
3. **Configure**: Access web interface to set camera parameters and sync timing
4. **Preview**: View real-time feeds from selected cameras
5. **Record**: Start synchronized capture across all 32 cameras
6. **Review**: Check captured frames in preview mode
7. **Process**: Convert raw SRGGB10P files to PNG/JPEG using debayer tool
8. **Analyze**: Use processed images for 3D reconstruction or analysis

## Technical Specifications

### Camera Specifications
- **Sensor**: Sony IMX296 Global Shutter
- **Resolution**: 1456×1088 pixels
- **Format**: SRGGB10P (10-bit Bayer packed)
- **Interface**: CSI-2 to Raspberry Pi 5

### Network Performance
- **Protocol**: WebSocket for real-time streaming
- **Topology**: Star network via managed switches
- **Throughput**: ~2 MB/frame × 32 cameras = 64 MB per capture cycle

### Synchronization Accuracy
- **Jitter**: Sub-microsecond (hardware-based PWM)
- **Resolution**: 1 MHz timer (1 µs precision)
- **Cable**: SMA coaxial for minimal signal degradation

## Applications

- **Motion Capture**: High-speed capture of dynamic objects
- **3D Reconstruction**: Multi-view photogrammetry
- **Sports Analysis**: Frame-by-frame motion breakdown
- **Scientific Imaging**: Synchronized multi-angle recording
- **Visual Effects**: Time-slice photography and bullet-time effects

## Related Projects

- **Camera Control Server**: [cherupi-v4l2](https://github.com/kennychufk/cherupi-v4l2)
- **Web Client Interface**: [telefacet-web](https://github.com/kennychufk/telefacet-web)
- **Trigger Generator**: [cherupi-pwm-trigger](https://github.com/kennychufk/cherupi-pwm-trigger)
- **Image Processing**: [cherupi-debayer](https://github.com/kennychufk/cherupi-debayer)
