# .cursorrules - C++/JUCE 8 Audio Plugins

**Version**: 1.0.0  
**Last Updated**: 2025-11-16  
**Context**: Audio Plugins, DSP, Real-time Systems  
**Tech Stack**: C++17-20, JUCE 8

---

## Project Context
This project uses C++17-20 standard with JUCE 8 framework for audio plugin development.
Focus areas: DSP algorithms, real-time audio processing, low-latency systems.

---

## Model Recommendations

### For This Context, Prefer:
- **Claude Sonnet 4.5** - Complex DSP logic, algorithm design, JUCE architecture
- **Composer 1** - Multi-file JUCE module refactoring, plugin structure changes
- **GPT-5.1 Codex** - Code generation, boilerplate creation
- **Claude Opus 4.1** - Mission-critical audio processing, architecture decisions

### Avoid:
- ❌ Fast/Low models for critical DSP code (accuracy matters)
- ❌ Models without code specialization for audio domain

### Budget-Friendly Options (Free/Unlimited):
- **Haiku 4.5** - Quick questions, simple code snippets, iterations (Unlimited, Very Fast)
- **GPT-5 Fast** - Quick code generation, simple refactoring (Unlimited, Fast)
- **GPT-5.1 Fast** - Latest GPT with fast responses (Unlimited, Fast)
- **GPT-5 Codex Fast** - Quick code completion, simple edits (Unlimited, Fast)
- **GPT-5.1 Codex Fast** - Latest codex with speed (Unlimited, Fast)

**When to Use Budget Models:**
- ✅ Simple JUCE boilerplate code
- ✅ Quick iterations and experiments
- ✅ Non-critical code changes
- ✅ Learning and exploration
- ✅ Code comments and documentation

**When NOT to Use Budget Models:**
- ❌ Critical DSP algorithms (accuracy matters)
- ❌ Real-time safety-critical code
- ❌ Complex JUCE architecture decisions
- ❌ Performance optimization

---

## Core Principles

### C++ Best Practices
- **RAII (Resource Acquisition Is Initialization)** - Always use smart pointers
- **const correctness** - Mark everything const that can be const
- **Move semantics** - Use std::move for expensive operations
- **No raw pointers** - Prefer unique_ptr, shared_ptr, or references
- **Exception safety** - Use RAII to guarantee exception safety

### JUCE 8 Specific
- **AudioProcessor base class** - Extend for audio plugins
- **AudioProcessorValueTreeState** - For parameter management
- **juce::dsp** - Use JUCE's DSP module for filters, effects
- **juce::AudioBuffer** - For audio data handling
- **juce::MemoryBlock** - For binary data

### Real-Time Constraints
- **No allocations in processBlock()** - Pre-allocate all buffers
- **Lock-free data structures** - Use atomics, lock-free queues
- **SIMD optimization** - Use juce::dsp::SIMDRegister for performance
- **Cache-friendly code** - Minimize memory access patterns
- **Predictable execution** - Avoid branches, virtual calls in hot paths

---

## Code Style

### Naming Conventions
- **Classes**: PascalCase (`AudioProcessor`, `FilterDesigner`)
- **Functions**: camelCase (`processBlock`, `prepareToPlay`)
- **Variables**: camelCase (`sampleRate`, `bufferSize`)
- **Constants**: UPPER_SNAKE_CASE (`MAX_BUFFER_SIZE`, `DEFAULT_SAMPLE_RATE`)
- **Member variables**: camelCase with `m_` prefix (`m_sampleRate`, `m_buffer`)
- **JUCE types**: Follow JUCE conventions (`juce::String`, `juce::AudioBuffer<float>`)

### File Organization
```
plugin/
├── Source/
│   ├── PluginProcessor.h/cpp    # Main audio processor
│   ├── PluginEditor.h/cpp       # UI component
│   ├── PluginParameters.h       # Parameter definitions
│   └── DSP/
│       ├── Filter.h/cpp         # DSP algorithms
│       └── Effects.h/cpp        # Audio effects
├── Resources/                   # UI resources
└── JuceLibraryCode/            # JUCE framework
```

---

## DSP Best Practices

### Audio Processing
```cpp
// ✅ GOOD: Pre-allocate buffers
void prepareToPlay(double sampleRate, int samplesPerBlock) override {
    m_buffer.setSize(getTotalNumInputChannels(), samplesPerBlock);
    m_dspChain.prepare({sampleRate, static_cast<uint32>(samplesPerBlock), 
                       static_cast<uint32>(getTotalNumInputChannels())});
}

// ✅ GOOD: Real-time safe processBlock
void processBlock(juce::AudioBuffer<float>& buffer, 
                  juce::MidiBuffer& midiMessages) override {
    juce::ScopedNoDenormals noDenormals;
    m_dspChain.process(juce::dsp::ProcessContextReplacing<float>(buffer));
}

// ❌ BAD: Allocation in processBlock
void processBlock(...) override {
    auto* tempBuffer = new float[samplesPerBlock]; // NO!
}
```

### Parameter Management
```cpp
// ✅ GOOD: Use AudioProcessorValueTreeState
m_apvts.createAndAddParameter("gain", "Gain", "dB", 
    juce::NormalisableRange<float>(-60.0f, 12.0f, 0.1f), 
    0.0f, nullptr, nullptr);

// ✅ GOOD: Thread-safe parameter access
float gain = *m_apvts.getRawParameterValue("gain");
```

---

## Error Handling

### Exception Safety
- Use RAII for all resources
- Prefer noexcept for functions that shouldn't throw
- Use std::optional for optional values
- Validate inputs in prepareToPlay(), not processBlock()

### JUCE Error Handling
```cpp
// ✅ GOOD: Check JUCE result types
auto result = m_formatManager.createReaderFor(file);
if (result == nullptr) {
    DBG("Failed to create audio reader");
    return false;
}
```

---

## Performance Optimization

### Critical Rules
1. **No virtual calls in processBlock()** - Use templates or function pointers
2. **Minimize memory allocations** - Pre-allocate in prepareToPlay()
3. **Use SIMD** - Leverage juce::dsp::SIMDRegister
4. **Cache-friendly access** - Sequential memory access patterns
5. **Avoid branches** - Use lookup tables for complex conditions

### Example: SIMD Optimization
```cpp
// ✅ GOOD: SIMD-optimized filter
void processSIMD(juce::AudioBuffer<float>& buffer) {
    auto* channelData = buffer.getWritePointer(0);
    juce::dsp::SIMDRegister<float> simdGain(m_gain);
    
    for (int i = 0; i < buffer.getNumSamples(); i += 4) {
        juce::dsp::SIMDRegister<float> samples(channelData + i);
        samples *= simdGain;
        samples.copyToRawArray(channelData + i);
    }
}
```

---

## Testing

### Unit Testing
- Test DSP algorithms with known input/output
- Use juce::UnitTest framework
- Test edge cases (silence, clipping, NaN, Inf)
- Performance benchmarks for real-time constraints

### Integration Testing
- Test with real audio files
- Verify latency requirements
- Test parameter automation
- MIDI handling verification

---

## Documentation

### Code Comments
- Document DSP algorithms (what, why, how)
- Explain non-obvious optimizations
- Note real-time constraints
- Reference papers for complex algorithms

### Example:
```cpp
/**
 * Implements a biquad filter using direct form II transposed.
 * 
 * This structure minimizes memory usage and is optimized for SIMD.
 * Real-time safe: no allocations, O(1) complexity per sample.
 * 
 * Reference: "Digital Signal Processing" by Proakis & Manolakis
 */
class BiquadFilter {
    // ...
};
```

---

## Common Patterns

### Plugin Initialization
```cpp
YourPluginAudioProcessor::YourPluginAudioProcessor()
    : AudioProcessor(BusesProperties()
        .withInput("Input", juce::AudioChannelSet::stereo(), true)
        .withOutput("Output", juce::AudioChannelSet::stereo(), true))
{
    // Initialize parameters
    // Set default values
    // Allocate buffers
}
```

### Parameter Automation
```cpp
// ✅ GOOD: Smooth parameter changes
void updateParameters() {
    m_gain.setTargetValue(*m_apvts.getRawParameterValue("gain"));
    m_gain.skip(samplesPerBlock); // Smooth interpolation
}
```

---

## Anti-Patterns to Avoid

❌ **Allocations in processBlock()**
```cpp
// BAD
void processBlock(...) {
    auto buffer = new float[samplesPerBlock]; // NO!
}
```

❌ **Virtual calls in hot paths**
```cpp
// BAD
void processBlock(...) {
    m_processor->process(buffer); // Virtual call in hot path
}
```

❌ **Locking in processBlock()**
```cpp
// BAD
void processBlock(...) {
    std::lock_guard<std::mutex> lock(m_mutex); // NO LOCKS!
}
```

---

## Resources

- **JUCE Documentation**: https://juce.com/learn/documentation
- **JUCE Tutorials**: https://juce.com/learn/tutorials
- **DSP Theory**: "The Scientist and Engineer's Guide to Digital Signal Processing"
- **Real-Time Systems**: "Real-Time Systems" by Jane Liu

---

## Model Selection Guide

| Task Type | Recommended Model | Budget Alternative | Reason |
|-----------|-------------------|-------------------|--------|
| **DSP Algorithm Design** | Claude Sonnet 4.5 | Haiku 4.5 (simple) | Best reasoning for complex math |
| **Multi-file Refactoring** | Composer 1 | GPT-5.1 Codex Fast | Efficient for structured changes |
| **JUCE Architecture** | Claude Opus 4.1 | Claude Sonnet 4.5 | Mission-critical decisions |
| **Code Generation** | GPT-5.1 Codex | GPT-5.1 Codex Fast | Specialized for code |
| **Quick Iterations** | Haiku 4.5 | GPT-5.1 Codex Low Fast | Fast, good enough for simple tasks |
| **Simple Boilerplate** | GPT-5.1 Codex | Haiku 4.5 | Quick code snippets |
| **Code Comments** | Haiku 4.5 | GPT-5 Low Fast | Documentation |

---

**Remember**: Audio processing is real-time critical. Always prioritize correctness and predictability over clever optimizations.

