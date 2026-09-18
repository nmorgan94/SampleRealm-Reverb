# SampleRealm: Reverb

A send-style reverb with a filtered send path and a live spectrum display

## Features

### Signal Chain

The dry signal is never filtered. Only the reverb send is:

```
dry ───────────────────────────────────────────┐
                                               ├─→ out
in ──→ highpass ──→ send gain ──→ reverb ──────┘
```

The reverb itself runs 100% wet, and the dry/wet blend is handled separately, so **Mix** stays predictable no matter how hard the send is driven.

### Highpass

- 24 dB/oct, two cascaded 12 dB/oct IIR stages
- 20 Hz to 20 kHz, default 1 kHz, skewed for finer control at the low end
- Cuts only what reaches the reverb, so the dry low end stays intact - the usual fix for a reverb that turns muddy on bass-heavy material
- Bypassed automatically at 20 Hz, so parking the knob fully left costs nothing

### Send Gain

- 0 to 2× (up to +6 dB), default 1.0
- Sets how hard the signal hits the reverb, independent of the wet/dry blend
- Applied after the highpass, so the filter shapes what gets driven

### Reverb

- Room Size, 0 to 1, default 0.80
- Damping, 0 to 1, default 0.80 - high-frequency absorption as the tail decays
- Width, 0 to 1, default 0.50 - stereo spread of the wet signal

### Mix

- Dry/wet blend, 0 to 1, default 0.50
- Linear crossfade: the dry signal falls as the wet rises

### Spectrum Analyzer

- 2048-point FFT, 512 display points, redrawn at 30 Hz
- Logarithmic frequency mapping across 20 Hz to 20 kHz
- Normalized per frame against its own peak, so it shows balance across the spectrum rather than absolute level

#### Spectrum Bypass

A `spectrumBypass` parameter clears the display and stops the audio thread writing to the FIFO, for hosts where the analyzer isn't worth the CPU. It is reachable through host automation only; there is no on-screen control.


## System Requirements

- macOS 11.0 (Big Sur) or later
- Universal 2 - runs natively on Intel and Apple Silicon
- VST3, AU, or Standalone
- Mono or stereo; the input layout must match the output

## Build Requirements

- CMake 3.25+
- A C++23-capable compiler
- Git
- macOS development environment for AU/Standalone/VST3 builds

JUCE is fetched automatically at configure time via CPM - there is no submodule to initialize.

## Building

**Debug Build:**
```bash
cmake --preset debug
cmake --build --preset debug
```

**Release Build:**
```bash
cmake --preset release
cmake --build --preset release
```

Run the standalone:

```bash
open build-debug/Reverb_artefacts/Debug/Standalone/Reverb.app
```

Builds copy the VST3 and AU into the local plugin folders automatically.

## Debugging in Xcode

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
3. Load your plugin in AudioPluginHost

