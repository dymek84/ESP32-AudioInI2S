# Quick Start: Choosing Your Audio Processing Method

## TL;DR

**Just want it to work?** 
- Do nothing! ArduinoFFT is the default.

**Want better performance?**
- Add `#define USE_GOERTZEL` before including the library.

---

## Method 1: In Your Sketch (Easiest)

Add ONE line at the very top of your `.ino` file:

```cpp
#define USE_GOERTZEL  // For faster processing
// OR
#define USE_ARDUINOFFT  // For flexibility (this is default)

#include <AudioAnalysis.h>
// ... rest of your code
```

## Method 2: In platformio.ini (Recommended for PlatformIO)

```ini
[env:your-board]
build_flags = -D USE_GOERTZEL
```

## Method 3: Globally in Header (Advanced)

Edit `AudioAnalysis.h` or `AudioFrequencyAnalysis.h` and uncomment your choice:

```cpp
// Uncomment ONE:
// #define USE_ARDUINOFFT
// #define USE_GOERTZEL
```

---

## Quick Comparison

| Feature | ArduinoFFT | Goertzel |
|---------|-----------|----------|
| **Speed** | ⭐⭐ | ⭐⭐⭐⭐ |
| **Flexibility** | ⭐⭐⭐⭐ | ⭐⭐ |
| **Memory Usage** | High | Low |
| **Best For** | Custom frequencies | Musical notes |
| **External Deps** | Yes (arduinoFFT) | No |

---

## Need More Info?

See [PROCESSING_METHODS.md](./PROCESSING_METHODS.md) for complete documentation.
