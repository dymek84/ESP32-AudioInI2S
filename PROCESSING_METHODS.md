# Audio Processing Methods

This library supports two different audio processing methods: **ArduinoFFT** and **Goertzel algorithm**. You can choose which one to use based on your needs.

## Methods Comparison

### ArduinoFFT (Default)
- **Pros:**
  - Full spectrum analysis
  - More flexible for general-purpose frequency analysis
  - Better for analyzing unknown frequencies
  - Works well with custom frequency ranges
  
- **Cons:**
  - Higher computational cost
  - More memory usage
  - Slower processing

### Goertzel Algorithm
- **Pros:**
  - Optimized for specific frequencies (musical notes)
  - Lower computational cost
  - Faster processing
  - Lower memory footprint
  
- **Cons:**
  - Limited to predefined frequency set (musical notes)
  - Less flexible for custom frequency analysis
  - Fixed BAND_SIZE (64 bands)

## How to Switch Between Methods

### Method 1: Define in Your Sketch (Before Including Headers)

Add ONE of these defines at the top of your sketch **BEFORE** including the library:

```cpp
// For ArduinoFFT (default):
#define USE_ARDUINOFFT
#include <AudioAnalysis.h>
// or
#include <AudioFrequencyAnalysis.h>

// For Goertzel:
#define USE_GOERTZEL
#include <AudioAnalysis.h>
// or
#include <AudioFrequencyAnalysis.h>
```

### Method 2: Define in platformio.ini

For PlatformIO users, add build flags:

```ini
[env:esp32-s3-devkitc-1]
platform = espressif32
board = esp32-s3-devkitc-1
framework = arduino
build_flags = 
    -D USE_ARDUINOFFT  ; For ArduinoFFT
    ; or
    ; -D USE_GOERTZEL  ; For Goertzel
lib_deps = 
    kosme/arduinoFFT@^2.0.0  ; Only needed for USE_ARDUINOFFT
```

### Method 3: Edit Header File Directly

You can also uncomment the desired option directly in the header files:
- `AudioAnalysis.h`
- `AudioFrequencyAnalysis.h`

Look for these lines near the top:

```cpp
// Choose audio processing method:
// Uncomment ONE of the following to select the processing method:
// #define USE_ARDUINOFFT  // Use ArduinoFFT library (traditional FFT approach)
// #define USE_GOERTZEL    // Use Goertzel algorithm (optimized for specific frequencies)
```

## Examples

### Example 1: Using ArduinoFFT (Default)

```cpp
#define USE_ARDUINOFFT  // Optional, it's the default
#include <AudioInI2S.h>
#include <AudioAnalysis.h>

#define SAMPLE_SIZE 1024
#define SAMPLE_RATE 44100
#define BAND_SIZE 8

AudioAnalysis audioInfo;
AudioInI2S mic(32, 25, 33, 27);
int32_t samples[SAMPLE_SIZE];

void setup() {
  Serial.begin(115200);
  mic.begin(SAMPLE_SIZE, SAMPLE_RATE);
  audioInfo.setNoiseFloor(10);
  audioInfo.normalize(true, 0, 255);
}

void loop() {
  mic.read(samples);
  audioInfo.computeFFT(samples, SAMPLE_SIZE, SAMPLE_RATE);
  audioInfo.computeFrequencies(BAND_SIZE);
  
  // Use the processed data
  for (int i = 0; i < BAND_SIZE; i++) {
    Serial.print(audioInfo.getBand(i));
    Serial.print(" ");
  }
  Serial.println();
}
```

### Example 2: Using Goertzel

```cpp
#define USE_GOERTZEL  // Select Goertzel method
#include <AudioInI2S.h>
#include <AudioAnalysis.h>

#define SAMPLE_SIZE 1024
#define SAMPLE_RATE 44100
#define BAND_SIZE 64  // Fixed for Goertzel (musical notes)

AudioAnalysis audioInfo;
AudioInI2S mic(32, 25, 33, 27);
int32_t samples[SAMPLE_SIZE];

void setup() {
  Serial.begin(115200);
  mic.begin(SAMPLE_SIZE, SAMPLE_RATE);
  audioInfo.setNoiseFloor(10);
  audioInfo.normalize(true, 0, 255);
}

void loop() {
  mic.read(samples);
  audioInfo.computeFFT(samples, SAMPLE_SIZE, SAMPLE_RATE);
  audioInfo.computeFrequencies(BAND_SIZE);
  
  // Use the processed data
  for (int i = 0; i < BAND_SIZE; i++) {
    Serial.print(audioInfo.getBand(i));
    Serial.print(" ");
  }
  Serial.println();
}
```

## Notes

- **Default Behavior**: If you don't define either option, the library will default to **USE_ARDUINOFFT**
- **Mutual Exclusion**: Only ONE method can be active at a time
- **Library Dependencies**: 
  - ArduinoFFT requires the `arduinoFFT` library to be installed
  - Goertzel is self-contained (no external dependencies)
- **BAND_SIZE**: With Goertzel, the bands correspond to musical notes and are predefined
- **Performance**: Goertzel is generally faster and uses less memory, making it ideal for resource-constrained applications

## Which Method Should I Use?

- **Use ArduinoFFT if:**
  - You need flexible frequency analysis
  - You want custom frequency ranges
  - You need variable band sizes
  - Processing speed is not critical

- **Use Goertzel if:**
  - You're working with musical applications
  - You need better performance
  - You have limited memory
  - You're analyzing specific frequencies (musical notes)
  - You want lower power consumption
