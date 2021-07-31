## 作業一: 作業1：練習將 DHT22 接於 GPIO 27 接腳，並且更改軟體的接腳設定值，重新執行範例程式，驗證在不同的接腳上安裝 DHT22，程式一樣可以正確地讀出數值。
- 由於 Adafruit_DHT 套件預設不認識 Pi4 因此要修改套件模組
```
root@raspberrypi:/home/pi# cd /
root@raspberrypi:/# cd /usr/local/lib/python3.7/dist-packages/Adafruit_DHT
root@raspberrypi:/usr/local/lib/python3.7/dist-packages/Adafruit_DHT# vim platform_detect.py
```

- 可以在最下面找到
```
if not match:
	# Couldn't find the hardware, assume it isn't a pi.
	return None
if match.group(1) == 'BCM2708'
	#Pi1
	return 1
elif match.group(1) == 'BCM2709':
	# Pi 2
	return 2
elif match.group(1) == 'BCM2835' :
	# Pi 3
	return 3
elif match.group(1) == 'BCM2837':
	# pi 3bt
	return 3

else:
# Something elsel, not a pi.
return None
```
- 在 else 前面加上
```
elif match.group(1) == 'BCM2711'
	# for pi4 2021/07/11
	return 3
```
- 存檔退出
- 編輯控制 DTH22 的 Python 作業檔案
```
root@raspberrypi:/home/pi# cat DHT22Reader.py
import Adafruit_DHT
DHT_SENSOR = Adafruit_DHT.DHT22
DHT_PIN = 4
for i in range(5):
    humidity, temperature = Adafruit_DHT.read_retry(DHT_SENSOR, DHT_PIN)
    if humidity is not None and temperature is not None:
        print("Temp={} Humidity={}".format(temperature, humidity))
    else:
        print("Failed to retrieve data from humidity sensor")
root@raspberrypi:/home/pi#
```
- 執行
```
root@raspberrypi:/home/pi# python3 DHT22Reader.py
Temp=32.0 Humidity=68.5999984741211
Temp=32.0 Humidity=68.80000305175781
Temp=31.899999618530273 Humidity=68.80000305175781
Temp=32.0 Humidity=68.80000305175781
Temp=32.0 Humidity=68.69999694824219
root@raspberrypi:/home/pi#
```

## 作業二: 觀察 RPi.GPIO 的程式碼，與 GPIOZero 程式碼對於繼電器控制上寫法的不同，如果我們要設定 GPIO 26, 19, 13, 6 四個接腳控制一個四路繼電器，練習實作一個 GPIOZero 四路繼電器的控制程式。
繼電器問題提問中

## 作業三: 將作業 1 與作業 2 結合，設定程式在溫度 10 度以下，打開 GPIO26，溫度 10 度以上到 20 度之間，控制 GPIO19，溫度 20 度到 30 度之間，控制 GPIO13，溫度在 30 度以上，控制 GPIO6，達成在不同的溫度區間時，控制不同的繼電器的需求。
繼電器問題提問中