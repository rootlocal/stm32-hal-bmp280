# stm32-hal-bmp280

~~~ c
#include "bmp280.h"

int main(void){
    BMP280_HandleTypedef bmp280;
    float pressure, temperature, humidity;
    char buf[260] = {0};
    
    bmp280_init_default_params(&bmp280.params);
    bmp280.addr = BMP280_I2C_ADDRESS_0;
    bmp280.i2c = &hi2c1;

    while (!bmp280_init(&bmp280, &bmp280.params)) {
        sprintf(buf, "BMP280\ninit failed");
        // ...
        HAL_Delay(2000);
    }

    bool bme280p = bmp280.id == BME280_CHIP_ID;
    sprintf(Led, "found %s\n", bme280p ? "BME280" : "BMP280");
    // ...
    
    while(1){
        while (!bmp280_read_float(&bmp280, &temperature, &pressure, &humidity)) {
            sprintf(buf, "Temp failed\n");
            // ... 
            HAL_Delay(2000);
        }
        
        double pressure_mmHg = (pressure * 0.00750062); ///< convert Pa to mmHg
        sprintf(buf, "P: %.2f mmHg\nTemp: %.2f C", pressure_mmHg, temperature);
        // ...

        if (bme280p) {
           sprintf(buf,"Hum: %.2f\n", humidity);
           // ...
        }
    }
    

}
~~~