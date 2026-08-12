# SampleRealm: Reverb

A professional audio reverb plugin built with JUCE framework, featuring a custom-designed user interface with rotary controls.

## Features

- **High-quality reverb processing** for adding space and depth to audio
- **Cross-platform support**: VST3, AU, and Standalone formats
- **Universal binary**: Supports both Intel (x86_64) and Apple Silicon (arm64) architectures

## Build Requirements

- CMake 3.25+
- A C++23-capable compiler
- Git
- macOS development environment for AU/Standalone/VST3 builds


## Building

### Debug

```bash
cmake --preset debug
cmake --build --preset debug
```

### Release

```bash
cmake --preset release
cmake --build --preset release
```

## Debugging in Xcode

To debug the plugin in Xcode with an executable:

### 1. Generate Xcode Project

```bash
cmake -B build-xcode -G Xcode
open build-xcode/Reverb.xcodeproj
```

### 2. Configure Debugging

1. Select your plugin target from the scheme dropdown
2. Go to **Product → Scheme → Edit Scheme** 
3. Click **Run** on the left sidebar
4. Under **Executable**, choose **Other** and navigate to executable.

### 3. Build and Run

1. Press **Cmd+B** to build the plugin
2. Press **Cmd+R** to run with AudioPluginHost
4. Load your plugin in AudioPluginHost

## Using the Plugin

### Using a DAW

Load the plugin in your preferred DAW (Logic Pro, Ableton Live, Reaper, etc.) from the standard plugin locations.





