## Restoring your Oak to Factory

 **This should not be required unless you are hacking the core, bootloader, testing beta releases, or messing with writing to the flash chip**

 **Requires a 3.3V (NOT 5V) USB to Uart Adapter - we recommend the CH340/CH340G type.**

 - Install python or ensure it is in your path
 - Download this repo and unzip to a folder
 - Open a command prompt in the folder
 - Connect your USB to Uart adapter to the Oak. Connect Pin3/RX to TX and Pin4/TX to RX, GND to GND.
 - Connect Pin2 to GND on the Oak and power it up via a good stable power supply or good USB port.
 - Run the following command, replacing YOUR_COM_PORT with the com port of your uart to usb adapter.
 - If you get any errors check your connections or try another platform.

You may need to install pyserial on some platforms.

```
python esptool.py --baud 115200 --port YOUR_COM_PORT write_flash -fs 32m 0x1000 blank.bin 0x2000 oak_restore_factory.bin 0x101000 blank.bin 0x102000 blank.bin 0x202000 blank.bin 
```
**This will not restore an Oak that has had its Particle Config overwritten** at 0x100000 and 0x201000 - a device where that has occured can be partially restored by this method but then will need its device id set via serial, and you'll need to have recorded it previously. Then connect at 115200 baud and send set\n40\n{"device-id":"123456789012345678901234"}\n where 123456789012345678901234 is replaced by your device-id. You can also send a raw POST to 192.168.0.1/set with the same JSON while connected to the AP of the Oak
