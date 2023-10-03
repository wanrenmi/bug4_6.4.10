# bug4_6.4.10
A kernel bug in usbhid_probe in drivers/hid/usbhid/hid-core.c
Due to the specific malformed usb descriptor file, the wrong logic inside the function usbhid_probe in drivers/hid/usbhid/hid-core.c causes it to keep probing with the probe() function and eventually keep calling the hid_parser_main function to print the information, which is stuck in an infinite loop and consumes system resources, which may trigger a denial-of-service attack. 
![image](https://github.com/wanrenmi/bug4_6.4.10/assets/42407501/36c1bfe2-fff6-413f-b154-4e8d7974364d)
The following is the input usb descriptor file:
![image](https://github.com/wanrenmi/bug4_6.4.10/assets/42407501/7641f0c9-7e1f-477b-a731-05dbdfa442cf)
Among them, the first 18 bytes are the device descriptor, and the latter is the config descriptor. Insert the above file as a real or simulated USB device into a host using the linux kernel (It is currently certain that kernel version <=6.4.10 will be affected by this vulnerability, the latest kernel version has not been tested, but because there is no such part of code update, so there is a high probability that this vulnerability also exists). The result of the vulnerability triggering situation is shown in the figure below:
![image](https://github.com/wanrenmi/bug4_6.4.10/assets/42407501/6741f34a-683a-4634-a358-7333e7db000f)
