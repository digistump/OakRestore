## Restoring your Oak to Factory

 **This should not be required unless you are hacking the core, bootloader, testing beta releases, or messing with writing to the flash chip**

 **Requires a 3.3V (NOT 5V) USB to Uart Adapter - we recommend the CH340/CH340G type.**

 - Install python or ensure it is in your path (or use the esptool.exe in this repo if on windows)
 - Download this repo and unzip to a folder
 - Open a command prompt in the folder
 - Connect your USB to Uart adapter to the Oak. Connect Pin3/RX to TX and Pin4/TX to RX, GND to GND.
 - Connect Pin2 to GND on the Oak and power it up via a good stable power supply or good USB port.
 - Run the following command, replacing YOUR_COM_PORT with the com port of your uart to usb adapter.
 - If you get any errors check your connections or try another platform.
 - After restoring, unclaim the Oak if you previously added it to the Particle Cloud - at build.particle.io
 - Re-run the intial setup procedure with the config app

You may need to install pyserial on some platforms.

```
python esptool.py --baud 115200 --port YOUR_COM_PORT write_flash -fs 32m 0x1000 blank.bin 0x2000 oaksetup_restore.bin 0x0081000 oakupdate_restore.bin 0x101000 blank.bin 0x102000 blank.bin 0x202000 blank.bin 
```
or on windows

```
esptool --baud 115200 --port YOUR_COM_PORT write_flash -fs 32m 0x1000 blank.bin 0x2000 oaksetup_restore.bin 0x0081000 oakupdate_restore.bin 0x101000 blank.bin 0x102000 blank.bin 0x202000 blank.bin 
```
**This will not restore an Oak that has had its Particle Config overwritten** at 0x100000 and 0x201000 - a device where that has occured can be partially restored by this method but then will need its device id set via serial, and you'll need to have recorded it previously. Then connect at 115200 baud and send set\n40\n{"device-id":"123456789012345678901234"}\n where 123456789012345678901234 is replaced by your device-id. You can also send a raw POST to 192.168.0.1/set with the same JSON while connected to the AP of the Oak

To get your device-id : access the Oak url http://192.168.0.1/device-id while connected to its AP.

## Force update an Oak

**To force an Oak to use the latest system version without using the OTA Update: **

**(PLEASE DO NOT do this without first trying the above and providing us with the debugging output - that output is essential to us making this work for everyone.)**

Download this file into the same folder as you have copied this repository (ignore the SSL warning, this was intended for device use only): https://oakota.digistump.com/firmware/firmware_v1.bin

Then run:
```
python esptool.py --baud 115200 --port YOUR_COM_PORT write_flash -fs 32m 0x1000 blank.bin 0x2000 firmware_v1.bin 0x101000 blank.bin 0x102000 blank.bin 0x202000 blank.bin 
```
or on windows

```
esptool --baud 115200 --port YOUR_COM_PORT write_flash -fs 32m 0x1000 blank.bin 0x2000 firmware_v1.bin 0x101000 blank.bin 0x102000 blank.bin 0x202000 blank.bin 
```