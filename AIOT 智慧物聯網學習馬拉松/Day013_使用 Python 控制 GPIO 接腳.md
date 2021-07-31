## 作業二: 實際練習 GPIOZero 控制 LED，確定單獨的 GPIO 控制 LED 亮跟暗交替閃爍完成，並且 PWM 控制 LED 明亮的完成後，嘗試依序改變led.value的值，分別設定 0.1, 0.3, 0.5, 0.7 觀察差異。
```
root@raspberrypi:/home/pi# cat pwmled.py
from gpiozero import PWMLED
from time import sleep

led = PWMLED(16)

while True:
    led.value = 0.1
    sleep(1)
    led.value = 0.3
    sleep(1)
    led.value = 0.5
    sleep(1)
    led.value = 0.7
    sleep(1)
root@raspberrypi:/home/pi#
```

### 作業三: 實際練習 GPIOZero 透過 Button 控制 LED，在按鈕的過程中，觀察實際按下按鈕的次數，LED 點亮的次數，是否一致。練習修改程式，讓按鈕按下是全亮，按鈕放開後是 30% 的亮度。
```
root@raspberrypi:/home/pi# cat button_led.py
from gpiozero import PWMLED, Button
from signal import pause

button = Button(17)
led = PWMLED(16)

def led_value():
    led.value = 0.3
def led_value_1():
    led.value = 1
button.when_released = led_value
button.when_pressed = led_value_1

pause()
root@raspberrypi:/home/pi#
```