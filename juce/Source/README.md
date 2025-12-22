# JUCE Source Directory

This directory contains the C++ source files for the audio plugin.

## Structure

```
Source/
├── PluginProcessor.h/cpp    # Main audio processor
├── PluginEditor.h/cpp       # UI component
├── PluginParameters.h       # Parameter definitions
└── DSP/                     # DSP algorithms and effects
    ├── Filter.h/cpp
    └── Effects.h/cpp
```

## Guidelines

- Follow the naming conventions in `juce/.cursorrules`
- Ensure all audio processing is real-time safe
- Pre-allocate buffers in `prepareToPlay()`
- No allocations in `processBlock()`
