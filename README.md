# ESP32-AudioInI2S
A simple MEMS I2S microphone and audio processing library for ESP32.

## Features
* Simple I2S sample reading and setup. Just choose the pins, sample size and sample rate.
* **Multiple Audio Processing Methods:** Choose between ArduinoFFT or Goertzel algorithm (see [PROCESSING_METHODS.md](./PROCESSING_METHODS.md))
* Robust audio processing classes for analysis.
  * Simple FFT/Goertzel compute on your I2S samples.
  * Frequency bands in 2, 4, 8, 16, 32 or 64 buckets.
  * Volume Unit Meter.
  * Set a noise floor to ignore values below it.
  * Normalize values into desired min/max ranges.
  * Auto level values for noisy/quiet enviroments where you want to keep values around the normalize max.
  * Ability to set the peak falloff rates and types. NO_FALLOFF, LINEAR_FALLOFF, ACCELERATE_FALLOFF, EXPONENTIAL_FALLOFF.
  * Equalizer to adjust the frequency levels for each bucket. Good for lowering the bass or treble response depending on the enviroment.

## Audio Processing Methods
This library supports two different audio processing approaches:
- **ArduinoFFT** (Default) - Full spectrum analysis, flexible but more resource-intensive
- **Goertzel** - Optimized for musical notes, faster and lower memory usage

See [PROCESSING_METHODS.md](./PROCESSING_METHODS.md) for detailed comparison and usage instructions.

#### [AudioInI2S Class README](./AudioInI2S.md)
  * [Basic](examples/Basic/Basic.ino) - Reads I2S microphone data to be viewed in the Serial Plotter.

#### [AudioFrequencyAnalysis Class README](./AudioFrequencyAnalysis.md) (New Way - Pick the frequencies range buckets you want)
  * [FrequencyRange](examples/FrequencyRange/FrequencyRange.ino) - Reads I2S microphone data, processes them into custom FrequencyRange buckets to be viewed in the Serial Plotter.
  * [FrequencyRange-Visuals](examples/TTGO-T-Display/FrequencyRange-Visuals/FrequencyRange-Visuals.ino) - Reads I2S microphone data, processes them into custom FrequencyRange buckets and displays them on a

#### [AudioAnalysis Class README](./AudioAnalysis.md) (Old Way - One Shot frequency buckets 20Hz - 20KHz)
  * [Frequencies](examples/Frequencies/Frequencies.ino) - Reads I2S microphone data, processes them into frequency buckets to be viewed in the Serial Plotter.
  * [FastLED](examples/FastLED/FastLED.ino) - Reads I2S microphone data, processes them into frequency buckets and displays them on a WS2812B led strip.
  * [OLED](examples/OLED_128x64/OLED_128x64.ino) - Reads I2S microphone data, processes them into frequency buckets and displays them on a 128x64 OLED display.
  * [TFT](examples/TTGO-T-Display/Basic-Visuals/Basic-Visuals.ino) - Reads I2S microphone data, processes them into frequency buckets and displays them on a 240x135 16bit TFT display.

240x135 16bit TFT display.

## Hardware 
* ESP32, ESP32 S2, ESP32 C2, ESP32 C3
* INMP441 - MEMS Microphone

## Known Issues
The `AudioAnalysis.h` and `AudioFrequencyAnalysis.h` classes support two processing methods:
- **ArduinoFFT** (default) - Built on top of ArduinoFFT V2 develop branch. You can find out more about it here: https://github.com/kosme/arduinoFFT/tree/develop
- **Goertzel** - Self-contained implementation optimized for musical note frequencies

`AudioAnalysis.h` is not optimized and uses a lot of helper variables and floats. That said it is still very responsive at 1024 sample size and 44100 sample rate.

For better performance on resource-constrained devices, consider using the Goertzel method (see [PROCESSING_METHODS.md](./PROCESSING_METHODS.md)).

## Recognition
This library's audio processing would not be possible without:
- ArduinoFFT https://github.com/kosme/arduinoFFT (when using ArduinoFFT method)
- Goertzel algorithm implementation (integrated for musical note analysis)

## Licensing 
MIT Open Source - Free to use anywhere. 
