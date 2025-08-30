# MS5611 STM32 HAL Driver

STM32 HAL driver for the **MS5611** high‑resolution barometric pressure and temperature sensor (I²C interface). **The library supports both types of modules.**

Thanks to [@RobTillaart](https://github.com/RobTillaart) for letting me use his [MS5611](https://github.com/RobTillaart/MS5611) library to create this library for STM32.

## How to use

Download the files and place them in your project accordingly. Then add `#include "ms5611.h"` to your file where you want to use the library functions.

## Function descriptions

#### `MS5611_Init(I2C_HandleTypeDef *hi2c, uint8_t mathMode)`
Initializes the MS5611 sensor with the specified I²C handle and math mode.

**Parameters:**
- `hi2c` – pointer to the HAL I²C handle (e.g. `&hi2c1`)
- `mathMode`:
  - `0` *(default)* – default atmospheric pressure calculation (datasheet constants)
  - `1` – modified atmospheric pressure calculation (Application Note constants)

> [!TIP]
> If you are unsure which `mathMode` to use, start with `0`.  
> If the measured pressure is unrealistic (e.g. ~498 hPa), try setting it to `1`.

#### `MS5611_Read(I2C_HandleTypeDef *hi2c, float *temperature, float *pressure)`
Reads temperature and pressure data from the sensor and stores them in the provided variables.

**Parameters:**
- `hi2c` – pointer to the HAL I²C handle
- `temperature` – pointer to a `float` to store temperature in °C
- `pressure` – pointer to a `float` to store pressure in Pa

> [!NOTE]
> The units returned by the function are **°C** and **Pa**

## Example code

```c
#include "main.h"
#include "i2c.h"
#include "ms5611.h"
#include <stdio.h>

I2C_HandleTypeDef hi2c1;

int main(void)
{
    /* Initialize the HAL library and system clock */
    HAL_Init();
    SystemClock_Config();

    /* Initialize I2C1 peripheral */
    MX_I2C1_Init();

    /* Initialize MS5611 sensor with mathMode = 0 (default) */
    MS5611_Init(&hi2c1, 0);

    while (1)
    {
        float temperature, pressure;

        /* Read temperature (°C) and pressure (Pa) from the sensor */
        MS5611_Measure(&hi2c1, &temperature, &pressure);

        /* Print results over UART (pressure converted to hPa) */
        printf("Temperature: %.2f °C\r\n", temperature);
        printf("Pressure: %.2f hPa\r\n", pressure / 100.0f);

        HAL_Delay(1000);
    }
}
```

## Additional information

If something isn't working for you or there's a bug with the library, feel free to contact me or create a new Issue.


