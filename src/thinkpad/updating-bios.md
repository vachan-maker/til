# Updating the BIOS

Please also refer to the references used to write these instructions
- [Tojaj's Blog](https://tojaj.com/lenovo-biosuefi-update-from-usb-stick-i-e-without-bootable-cd/)
- [nixCraft](https://www.cyberciti.biz/faq/update-lenovo-bios-from-linux-usb-stick-pen/)

<div class = "warning">
You should always be careful when updating the BIOS. I will not be responsible for any damage caused to your system. Always take the necessary precautions and follow the instructions  carefully.
</div>
If you are on Windows, you can easily update the BIOS with Lenovo Vantage.

But on Linux distributions, Lenovo Vantage is not supported and since Thinkpad 16 G2 (AMD) is not Linux certified, you cannot get udate the BIOS using fwupd.

You can manually update the BIOS by downloading the image and flashing it on to a USB Drive.

1. Download the BIOS image from [Lenovo Support](https://support.lenovo.com). Make sure you choose the correct model. You should download the CD ISO image.
2. Download [geteltorito](https://github.com/rainer042/geteltorito)
3. Change directory to geteltorito
4. Set permissions by using the below command

    `chmod +x ./geteltorito.pl`
5. Extract the bootable image from the ISO:

    `./geteltorito.pl -o bios.img iso_file.iso`
<div class = "warning">The below command will delete everything in your USB Drive. Make sure you don't have any important files in it</div>
<div class = "warning"> When copying the image in your USB stick, make sure you have chosen the correct device</div>
6. Copy the image into your USB stick by dd utility

    sudo dd if=bios.img of=/dev/sdb
You now have a bootable USB drive.

**Secure boot must be disabled it for the computer to boot the USB drive**