# Experimental Results

## Objective

To develop a portable digital stethoscope for heartbeat monitoring using LPC2148 microcontroller.

---

## Hardware Implementation

The hardware system was assembled using:

- LPC2148 ARM7 Development Board  
- MAX4466 Microphone Module  
- SSD1306 OLED Display  
- Speaker Output Circuit  
- Stethoscope Chest Piece  

---

## Heart Sound Detection

- Stethoscope chest piece successfully captured body sound vibrations.  
- MAX4466 microphone converted sound into analog electrical signal.  
- Signal was sent to LPC2148 ADC input.

---

## ADC Processing

- LPC2148 ADC continuously sampled incoming analog signal.  
- Analog signal converted into 10-bit digital values.  
- Real-time ADC values were processed successfully.

---

## BPM Calculation

- Heartbeat peaks detected using threshold comparison.  
- BPM calculated using time interval measurement.

Formula:

BPM = 60000 / Time Interval(ms)

---

## OLED Display Result

OLED successfully displayed:

- BPM value  
- ADC value  
- Heartbeat waveform  

---

## Experimental Output

Final hardware prototype:

![Final Output](result.jpeg)

---

## Final Performance

The system successfully demonstrated:

- Heart sound acquisition  
- ADC conversion  
- BPM calculation  
- OLED waveform visualization  
- Portable digital stethoscope operation  

---

## Conclusion

The Digital Stethoscope system successfully monitored heartbeat signals and displayed real-time waveform and BPM using LPC2148 ARM7 microcontroller.
