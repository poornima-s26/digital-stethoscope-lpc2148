# Digital Stethoscope Source Code

---

## ADC.c

```c
#include <LPC214x.h>
#include "ADC.h"

void init_adc(void) {
    PINSEL1 |= (1 << 26); // P0.28 as AD1.1
    AD1CR = (1 << 1) | (1 << 8) | (3 << 17) | (1 << 21);
}

int read_adc(void) {
    AD1CR |= (1 << 24); // Start conversion
    while (!(AD1GDR & (1 << 31))); // Wait for conversion complete
    return (AD1GDR >> 6) & 0x3FF;  // Return 10-bit result
}
```

---

## ADC.h

```c
#define ADC_H
#define ADC_H

void ADC_Init(void);
int ADC_Read(void);
```

---

## i2c.c

```c
#include <LPC214x.h>
#include "i2c.h"

void i2c_init(void) {
    PINSEL0 |= 0x00000050; // P0.2=SDA, P0.3=SCL
    I2C0SCLH = 60;
    I2C0SCLL = 60;
    I2C0CONCLR = 0x6C;
    I2C0CONSET = 0x40;
}

void i2c_start(void) {
    I2C0CONSET = 0x20;
    while (!(I2C0CONSET & 0x08));
    I2C0CONCLR = 0x28;
}

void i2c_stop(void) {
    I2C0CONSET = 0x10;
    I2C0CONCLR = 0x08;
}

void i2c_write(unsigned char data) {
    I2C0DAT = data;
    I2C0CONCLR = 0x08;
    while (!(I2C0CONSET & 0x08));
}
```

---

## i2c.h

```c
#define I2C_H
#define I2C_H

void I2C_Init(void);
void I2C_Start(void);
void I2C_Stop(void);
void I2C_Write(unsigned char data);
```

---

## main.c

```c
#include <LPC214x.h>
#include "adc.h"
#include "i2c.h"
#include "ssd1306.h"

void delay_ms(unsigned int ms) {
    unsigned int i;
    for (i = 0; i < ms * 6000; i++);
}

int main(void) {
    unsigned int adc_value;
    char buffer[20];
    int prev_adc = 0;
    int bpm = 0;
    unsigned int last_beat_time = 0;
    unsigned int current_time = 0;
    int waveform_x = 0;
    int y;
    int y_pos;

    init_adc();
    i2c_init();
    ssd1306_init();

    ssd1306_clear();
    ssd1306_string(0, 0, "Stethoscope", 1);

    while (1) {
        adc_value = read_adc(1);

        for (y = 2; y < 8; y++) {
            ssd1306_char(waveform_x, y, ' ', 1);
        }

        y_pos = 7 - (adc_value * 5 / 1023);
        ssd1306_char(waveform_x, y_pos, '*', 1);

        waveform_x++;
        if (waveform_x > 127) waveform_x = 0;

        current_time += 500;

        if (adc_value > 600 && prev_adc <= 600) {
            if (last_beat_time != 0) {
                unsigned int interval = current_time - last_beat_time;
                bpm = 60000 / interval;
            }
            last_beat_time = current_time;
        }

        prev_adc = adc_value;

        sprintf(buffer, "BPM: %3d", bpm);
        ssd1306_string(0, 1, buffer, 1);

        delay_ms(500);
    }
}
```

---

## ssd1306.c

```c
#include "ssd1306.h"
#include "i2c.h"
#include "font6x8.h"

#define SSD1306_ADDR 0x78

void ssd1306_cmd(unsigned char cmd) {
    i2c_start();
    i2c_write(SSD1306_ADDR);
    i2c_write(0x00);
    i2c_write(cmd);
    i2c_stop();
}

void ssd1306_init(void) {
    ssd1306_cmd(0xAE);
    ssd1306_cmd(0xAF);
}

void ssd1306_clear(void) {
    // OLED clear function
}

void ssd1306_char(unsigned char x, unsigned char y, char chr, unsigned char size) {
    // Character display
}

void ssd1306_string(unsigned char x, unsigned char y, char *str, unsigned char size) {
    // String display
}
```

---

## ssd1306.h

```c
#define SSD1306_H
#define SSD1306_H

void OLED_Init(void);
void OLED_WriteCmd(unsigned char cmd);
void OLED_Clear(void);
void OLED_DrawWaveform(int val);

void ssd1306_draw_waveform(int *data, int length);
```

---

## font6x8.h

```c
#ifndef FONT6X8_H
#define FONT6X8_H

const unsigned char font6x8[][6] = {
  {0x00,0x00,0x00,0x00,0x00,0x00},
  {0x00,0x00,0x5F,0x00,0x00,0x00}
  // remaining font array...
};

#endif
```
