## 作業一: 實際練習 /sys/class/gpio 啟動 gpio，設定 gpio 接腳的狀態，並且卸載所啟動的 gpio。同時觀察卸載之後的 gpio 接腳，繼續送設定狀態的資料，將會發生什麼樣的狀態。
```
root@raspberrypi:/home/pi# cat /sys/kernel/debug/gpio | grep gpio-4
 gpio-4   (GPIO_GCLK           )
 gpio-40  (PWM0_MISO           )
 gpio-41  (PWM1_MOSI           )
 gpio-42  (STATUS_LED_G_CLK    |led0                ) out lo
 gpio-43  (SPIFLASH_CE_N       )
 gpio-44  (SDA0                )
 gpio-45  (SCL0                )
 gpio-46  (RGMII_RXCLK         )
 gpio-47  (RGMII_RXCTL         )
 gpio-48  (RGMII_RXD0          )
 gpio-49  (RGMII_RXD1          )
root@raspberrypi:/home/pi# echo 4 > /sys/class/gpio/export
root@raspberrypi:/home/pi# cat /sys/kernel/debug/gpio | grep gpio-4
 gpio-4   (GPIO_GCLK           |sysfs               ) out hi
 gpio-40  (PWM0_MISO           )
 gpio-41  (PWM1_MOSI           )
 gpio-42  (STATUS_LED_G_CLK    |led0                ) out lo
 gpio-43  (SPIFLASH_CE_N       )
 gpio-44  (SDA0                )
 gpio-45  (SCL0                )
 gpio-46  (RGMII_RXCLK         )
 gpio-47  (RGMII_RXCTL         )
 gpio-48  (RGMII_RXD0          )
 gpio-49  (RGMII_RXD1          )
root@raspberrypi:/home/pi# echo 0 > /sys/class/gpio/gpio4/value
root@raspberrypi:/home/pi# cat /sys/kernel/debug/gpio | grep gpio-4
 gpio-4   (GPIO_GCLK           |sysfs               ) out lo
 gpio-40  (PWM0_MISO           )
 gpio-41  (PWM1_MOSI           )
 gpio-42  (STATUS_LED_G_CLK    |led0                ) out lo
 gpio-43  (SPIFLASH_CE_N       )
 gpio-44  (SDA0                )
 gpio-45  (SCL0                )
 gpio-46  (RGMII_RXCLK         )
 gpio-47  (RGMII_RXCTL         )
 gpio-48  (RGMII_RXD0          )
 gpio-49  (RGMII_RXD1          )
root@raspberrypi:/home/pi# echo 4 > /sys/class/gpio/unexport
root@raspberrypi:/home/pi# echo 0 > /sys/class/gpio/gpio4/value
bash: /sys/class/gpio/gpio4/value: No such file or directory
root@raspberrypi:/home/pi#
```

## 作業二: 使用 raspi-config 啟動 i2c，觀察 gpio2 以及 gpio3 的變化，透過 /sys/kernel/debug/gpio 觀察改變的情形，嘗試重新做一次作業1，針對 gpio2 以及 gpio3 操作，觀察在 i2c 啟動的狀態下，gpio2 以及 gpio3 相對 gpio4 有何不同。
```
root@raspberrypi:/home/pi# cat /sys/kernel/debug/gpio | grep gpio-2
 gpio-2   (SDA1                )
 gpio-20  (GPIO20              )
 gpio-21  (GPIO21              )
 gpio-22  (GPIO22              )
 gpio-23  (GPIO23              )
 gpio-24  (GPIO24              )
 gpio-25  (GPIO25              )
 gpio-26  (GPIO26              )
 gpio-27  (GPIO27              )
 gpio-28  (RGMII_MDIO          )
 gpio-29  (RGMIO_MDC           )
root@raspberrypi:/home/pi# cat /sys/kernel/debug/gpio | grep gpio-3
 gpio-3   (SCL1                )
 gpio-30  (CTS0                )
 gpio-31  (RTS0                )
 gpio-32  (TXD0                )
 gpio-33  (RXD0                )
 gpio-34  (SD1_CLK             )
 gpio-35  (SD1_CMD             )
 gpio-36  (SD1_DATA0           )
 gpio-37  (SD1_DATA1           )
 gpio-38  (SD1_DATA2           )
 gpio-39  (SD1_DATA3           )
root@raspberrypi:/home/pi# raspi-config
root@raspberrypi:/home/pi# cat /sys/kernel/debug/gpio | grep gpio-2
 gpio-2   (SDA1                )
 gpio-20  (GPIO20              )
 gpio-21  (GPIO21              )
 gpio-22  (GPIO22              )
 gpio-23  (GPIO23              )
 gpio-24  (GPIO24              )
 gpio-25  (GPIO25              )
 gpio-26  (GPIO26              )
 gpio-27  (GPIO27              )
 gpio-28  (RGMII_MDIO          )
 gpio-29  (RGMIO_MDC           )
root@raspberrypi:/home/pi# cat /sys/kernel/debug/gpio | grep gpio-3
 gpio-3   (SCL1                )
 gpio-30  (CTS0                )
 gpio-31  (RTS0                )
 gpio-32  (TXD0                )
 gpio-33  (RXD0                )
 gpio-34  (SD1_CLK             )
 gpio-35  (SD1_CMD             )
 gpio-36  (SD1_DATA0           )
 gpio-37  (SD1_DATA1           )
 gpio-38  (SD1_DATA2           )
 gpio-39  (SD1_DATA3           )
root@raspberrypi:/home/pi# echo 2 > /sys/class/gpio/gpio2/value
bash: /sys/class/gpio/gpio2/value: No such file or directory
root@raspberrypi:/home/pi# echo 2 > /sys/class/gpio/export
root@raspberrypi:/home/pi# cat /sys/kernel/debug/gpio | grep gpio-2
 gpio-2   (SDA1                |sysfs               ) in  hi
 gpio-20  (GPIO20              )
 gpio-21  (GPIO21              )
 gpio-22  (GPIO22              )
 gpio-23  (GPIO23              )
 gpio-24  (GPIO24              )
 gpio-25  (GPIO25              )
 gpio-26  (GPIO26              )
 gpio-27  (GPIO27              )
 gpio-28  (RGMII_MDIO          )
 gpio-29  (RGMIO_MDC           )
root@raspberrypi:/home/pi#
```

## 作業三: 使用 raspi-config 啟動 spi，觀察 gpio 各接腳的變化狀態，嘗試將 spi 關閉之後，透過 /sys/kernel/debug/gpio，觀察可以使用 gpio 數量的變化情形 。
```
root@raspberrypi:/home/pi# raspi-config
root@raspberrypi:/home/pi# cat /sys/kernel/debug/gpio | grep gpio-7
 gpio-7   (SPI_CE1_N           |spi0 CS1            ) out hi ACTIVE LOW
root@raspberrypi:/home/pi# raspi-config
root@raspberrypi:/home/pi# cat /sys/kernel/debug/gpio | grep gpio-7
 gpio-7   (SPI_CE1_N           )
root@raspberrypi:/home/pi#
```