## Booting Orange PI 5 from NVMe SSD

Booting directly from NVMe SSD only works after installing the bootloader on SPI Flash.
<br>· In order to do this, First install the OS to my SD card, and boot</br>
```
Steps as Follows :

1. Use etcher (or your preferred imaging tool) to write the OS image to an SD card
2. Use etcher again to write the same image to your NVMe SSD <-- Duplicate frst step to SSD
3. Insert both the SD card and the NVMe SSD into the Orange Pi 5
4. Power on the Orange Pi (the first time around it will boot from the SD card)
5. SSH into the Orange Pi 5, and run: orangepi-config (or armbian-install if you're using armbian)
5. Select boot options, then select: Install/Update the bootloader on SPI Flash (To install Bootloader)
```
## It may take a few minutes to finish installing the bootloader.
Once this is complete, Remove the SD card and the device will now be able to boot directly from NVMe
<br>(SD card is no longer needed -- this is true even if you decide to re-image your NVMe SSD)</br>
