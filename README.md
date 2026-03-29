# STM32-ADC-Interrupt-Temperature-Monitor
A real-time temperature monitoring system using STM32, where ADC runs in interrupt mode to continuously read analog data and send temperature values over UART.
# STM32 ADC Interrupt Temperature Monitor

## About this project

This project is a simple real-time temperature monitoring system built using STM32.

Instead of continuously checking the ADC in a loop, I used **interrupt-based ADC**. This makes the system more efficient because the CPU does not waste time waiting for conversion.

Whenever a conversion is complete, an interrupt is triggered, and the data is processed automatically.

---

## What it does

* Reads analog signal using ADC
* Uses **interrupt mode (non-blocking)**
* Converts ADC value into temperature
* Sends temperature data over UART
* Displays output in serial terminal (PuTTY)

---

## How it works (simple)

1. ADC starts in interrupt mode
2. Conversion completes → interrupt occurs
3. Inside interrupt:

   * ADC value is read
   * Converted into temperature
   * Flag is set
4. Main loop:

   * Checks flag
   * Sends temperature via UART

So basically:

ADC → Interrupt → Process Data → UART Output

---

## Output

In serial terminal:

Temperature: 25.34 C
Temperature: 25.60 C
Temperature: 25.12 C

(repeats every second)

---

## Configuration

### ADC

* Resolution → 12-bit
* Channel → ADC Channel 10
* Mode → Interrupt mode

### UART

* Baud rate → 115200
* TX → PA2
* RX → PA3

---

## Important Code Concept

### ADC Interrupt Callback

```c
void HAL_ADC_ConvCpltCallback(ADC_HandleTypeDef* hadc1)
{
    value = HAL_ADC_GetValue(hadc1);
    temp = value * 0.0805;
    data_ready = 1;

    HAL_ADC_Start_IT(hadc1);
}
```

---

## Why this project is useful

This project helps in understanding:

* ADC interrupt handling
* Real-time data acquisition
* Efficient CPU usage
* UART communication

Compared to polling, interrupt-based systems are more efficient and scalable.

---

## Limitations

* Uses simple scaling (not calibrated)
* Blocking UART transmission
* No filtering (noise possible)

---

## Future improvements

* Use DMA for ADC
* Add sensor calibration
* Implement averaging/filtering
* Display data on LCD
* Use RTOS for better task handling

---

## Final note

This project helped me understand how to build **real-time embedded systems using interrupts**.

Once you understand this, you can build applications like:

* temperature monitoring systems
* sensor data logging
* IoT-based monitoring
* industrial automation systems

---
