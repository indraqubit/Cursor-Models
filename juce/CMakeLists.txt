cmake_minimum_required(VERSION 3.15)

project(AudioPlugin VERSION 1.0.0)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Add JUCE
# Assumes JUCE is in a subdirectory or installed system-wide
# Adjust path as needed
# find_package(JUCE CONFIG REQUIRED)

# Or use FetchContent to download JUCE
# include(FetchContent)
# FetchContent_Declare(
#   JUCE
#   GIT_REPOSITORY https://github.com/juce-framework/JUCE.git
#   GIT_TAG        8.0.0
# )
# FetchContent_MakeAvailable(JUCE)

# Create the plugin target
# juce_add_plugin(AudioPlugin
#     COMPANY_NAME "YourCompany"
#     PLUGIN_MANUFACTURER_CODE Manu
#     PLUGIN_CODE Demo
#     FORMATS VST3 AU Standalone
#     PRODUCT_NAME "Audio Plugin"
# )

# Add source files
# target_sources(AudioPlugin
#     PRIVATE
#         Source/PluginProcessor.cpp
#         Source/PluginProcessor.h
#         Source/PluginEditor.cpp
#         Source/PluginEditor.h
# )

# Add compile definitions
# target_compile_definitions(AudioPlugin
#     PUBLIC
#         JUCE_WEB_BROWSER=0
#         JUCE_USE_CURL=0
#         JUCE_VST3_CAN_REPLACE_VST2=0
# )

# Link with JUCE modules
# target_link_libraries(AudioPlugin
#     PRIVATE
#         juce::juce_audio_utils
#         juce::juce_dsp
#     PUBLIC
#         juce::juce_recommended_config_flags
#         juce::juce_recommended_lto_flags
#         juce::juce_recommended_warning_flags
# )

# Enable testing
enable_testing()

# Add tests subdirectory if exists
# add_subdirectory(Tests)
