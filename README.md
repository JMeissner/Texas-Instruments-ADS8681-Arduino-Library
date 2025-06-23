# ADS8681 Arduino Library

A lightweight Arduino library for interfacing with the **Texas Instruments ADS8681**, a 16-bit, single-channel, bipolar input ADC with integrated features for high-accuracy data acquisition.

[Product Page on TI.com](https://www.ti.com/product/de-de/ADS8681)

## Features

- Supports bipolar input voltage ranges
- Range selection supported
- Manual control over ADC conversion and read timing
- SPI-based communication
- Easy initialization and usage

---

## Installation

1. Clone this repository:
   ```bash
   git clone https://github.com/JMeissner/Texas-Instruments-ADS8681-Arduino-Library.git
   ```

2. Copy it to your Arduino libraries folder.

---

## Usage

### Initialization

To create an ADS8681 object, specify the Chip Select (CS) pin:

```cpp
#include "ADS8681.h"

#define ADC_CS_PIN 10
ADS8681 adc(ADC_CS_PIN);
```

### Select Input Range

Use one of the predefined range macros from ADS8681.h. For example:

```cpp
adc.selectRange(ADS8681_RANGE_SEL_UP_1_25_VREF);
```

Available range macros include:
- `ADS8681_RANGE_SEL_UP_1_25_VREF`
- `ADS8681_RANGE_SEL_BP_1_25_VREF`
- `ADS8681_RANGE_SEL_UP_2_5_VREF`
- `ADS8681_RANGE_SEL_BP_2_5_VREF`

*(Refer to ADS8681.h for the full list)*

### Perform ADC Measurement

```cpp
// Select ADC
digitalWrite(ADC_CS_PIN, LOW);
delayMicroseconds(1);

// Start conversion by toggling CS/CONVST high
digitalWrite(ADC_CS_PIN, HIGH);
delayMicroseconds(1);  // Short pulse for CONVST
digitalWrite(ADC_CS_PIN, LOW);  // Return to CS active (low)

// Wait for ADC to finish conversion and read value
delayMicroseconds(ADC_DELAY);
uint16_t value = adc.adcRead();

// Deselect ADC
digitalWrite(ADC_CS_PIN, HIGH);
```

---

## Example

```cpp
#include "ADS8681.h"

#define ADC_CS_PIN 10
ADS8681 adc(ADC_CS_PIN);

void setup() {
  Serial.begin(9600);
  adc.selectRange(ADS8681_RANGE_SEL_UP_1_25_VREF);
}

void loop() {
  // Manual conversion
  digitalWrite(ADC_CS_PIN, LOW);
  delayMicroseconds(1);
  digitalWrite(ADC_CS_PIN, HIGH);
  delayMicroseconds(1);
  digitalWrite(ADC_CS_PIN, LOW);
  delayMicroseconds(10); // Adjust ADC_DELAY as needed
  
  uint16_t value = adc.adcRead();
  digitalWrite(ADC_CS_PIN, HIGH);
  
  Serial.println(value);
  delay(1000);
}
```

## Contribution
This project is largely based on the discontinued work from Adam Procio (Sponge5): https://github.com/Sponge5/ADS8681
