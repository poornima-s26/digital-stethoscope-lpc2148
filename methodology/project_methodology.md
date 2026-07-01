# Project Methodology

## Step 1 – Heart Sound Acquisition

- Stethoscope chest piece captures heart sound vibrations.
- Sound is transferred to microphone sensor.

## Step 2 – Signal Conversion

- MAX4466 microphone converts sound into analog electrical signal.
- Weak signal is sent to LPC2148 ADC input.

## Step 3 – Analog to Digital Conversion

- LPC2148 ADC continuously samples analog heart sound signal.
- 10-bit ADC converts analog waveform into digital values.

## Step 4 – Signal Processing

- Microcontroller processes ADC values.
- Threshold detection algorithm identifies heartbeats.

## Step 5 – BPM Calculation

- Time interval between consecutive heartbeats is measured.
- BPM calculated using:

BPM = 60000 / Time Interval(ms)

## Step 6 – OLED Display

- SSD1306 OLED connected through I2C communication.
- OLED displays:

  - BPM value
  - ADC value
  - Heartbeat waveform

## Step 7 – Continuous Monitoring

- System continuously updates heart rate and waveform in real time.

## Final Output

Portable digital stethoscope capable of real-time heartbeat monitoring and visualization.
