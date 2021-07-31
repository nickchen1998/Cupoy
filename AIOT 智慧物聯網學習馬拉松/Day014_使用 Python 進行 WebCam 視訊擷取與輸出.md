## 作業一: 執行 lsusb -v 指令，觀察系統顯示的 usb 裝置，透過 grep “14 Video” 指令篩選顯示的結果，了解 webcam 裝置在系統層次支援的狀態。
```
root@raspberrypi:/home/pi# lsusb -v | grep "14 Video"
can't get debug descriptor: Resource temporarily unavailable
      bFunctionClass         14 Video
      bInterfaceClass        14 Video
      bInterfaceClass        14 Video
      bInterfaceClass        14 Video
      bInterfaceClass        14 Video
      bInterfaceClass        14 Video
      bInterfaceClass        14 Video
      bInterfaceClass        14 Video
      bInterfaceClass        14 Video
      bInterfaceClass        14 Video
      bInterfaceClass        14 Video
      bInterfaceClass        14 Video
      bInterfaceClass        14 Video
      bInterfaceClass        14 Video
can't get debug descriptor: Resource temporarily unavailable
can't get debug descriptor: Resource temporarily unavailable
can't get device qualifier: Resource temporarily unavailable
can't get debug descriptor: Resource temporarily unavailable
root@raspberrypi:/home/pi#
```

## 作業二: 作業2：安裝 fswebcam，執行 fswebcam 拍一張照片，確定 webcam 動作正常，並且透過更改參數與設定參數檔案的方式，執行 fswebcam，確定可以產生隨時間依序儲存的檔案。
```
root@raspberrypi:/home/pi# fswebcam -r --l 5 --jpeg 50 --save /home/pi/webcamtest/%H%M%S.jpg
--- Opening /dev/video0...
Trying source module v4l2...
/dev/video0 opened.
No input was specified, using the first.
Adjusting resolution from 1x-1 to 160x120.
--- Capturing frame...
Captured frame in 0.00 seconds.
--- Processing captured image...
Writing JPEG image to '5'.
Setting output format to JPEG, quality 50
Writing JPEG image to '/home/pi/webcamtest/192309.jpg'.
root@raspberrypi:/home/pi# cd webcamtest/
root@raspberrypi:/home/pi/webcamtest# ls
192049.jpg  192309.jpg
root@raspberrypi:/home/pi/webcamtest#
```

## 作業三: 
- python 程式檔
```
root@raspberrypi:/home/pi/webcamtest# cat webcam_hw.py
import time
import os

x = 0
while x != 5:
    os.system("fswebcam -r 1920x1080 --jpeg 50 --save /home/pi/webcamtest/%d%m_test{}.jpg".format(x))
    x += 1
    time.sleep(5)
root@raspberrypi:/home/pi/webcamtest#
```
- 運行
```
root@raspberrypi:/home/pi/webcamtest# python3 webcam_hw.py
--- Opening /dev/video0...
Trying source module v4l2...
/dev/video0 opened.
No input was specified, using the first.
Adjusting resolution from 1920x1080 to 1280x960.
--- Capturing frame...
Captured frame in 0.00 seconds.
--- Processing captured image...
Setting output format to JPEG, quality 50
Writing JPEG image to '/home/pi/webcamtest/1107_test0.jpg'.
--- Opening /dev/video0...
Trying source module v4l2...
/dev/video0 opened.
No input was specified, using the first.
Adjusting resolution from 1920x1080 to 1280x960.
--- Capturing frame...
Captured frame in 0.00 seconds.
--- Processing captured image...
Setting output format to JPEG, quality 50
Writing JPEG image to '/home/pi/webcamtest/1107_test1.jpg'.
--- Opening /dev/video0...
Trying source module v4l2...
/dev/video0 opened.
No input was specified, using the first.
Adjusting resolution from 1920x1080 to 1280x960.
--- Capturing frame...
Captured frame in 0.00 seconds.
--- Processing captured image...
Setting output format to JPEG, quality 50
Writing JPEG image to '/home/pi/webcamtest/1107_test2.jpg'.
--- Opening /dev/video0...
Trying source module v4l2...
/dev/video0 opened.
No input was specified, using the first.
Adjusting resolution from 1920x1080 to 1280x960.
--- Capturing frame...
Captured frame in 0.00 seconds.
--- Processing captured image...
Setting output format to JPEG, quality 50
Writing JPEG image to '/home/pi/webcamtest/1107_test3.jpg'.
--- Opening /dev/video0...
Trying source module v4l2...
/dev/video0 opened.
No input was specified, using the first.
Adjusting resolution from 1920x1080 to 1280x960.
--- Capturing frame...
Captured frame in 0.00 seconds.
--- Processing captured image...
Setting output format to JPEG, quality 50
Writing JPEG image to '/home/pi/webcamtest/1107_test4.jpg'.
root@raspberrypi:/home/pi/webcamtest# ls
1107_test0.jpg  1107_test2.jpg  1107_test4.jpg  192309.jpg
1107_test1.jpg  1107_test3.jpg  192049.jpg      webcam_hw.py
root@raspberrypi:/home/pi/webcamtest#
```
- 察看結果
```
root@raspberrypi:/home/pi/webcamtest# ls
1107_test0.jpg  1107_test2.jpg  1107_test4.jpg  192309.jpg
1107_test1.jpg  1107_test3.jpg  192049.jpg      webcam_hw.py
root@raspberrypi:/home/pi/webcamtest#
```
