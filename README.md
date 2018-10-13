# How to set up a SLP440 on Ubuntu and Linux Mint (as of Oct 2018)

This was my adventure to set up this printer on Linux: trying to print a label with the `Smart Label Printer 4400 (SLP440)` by `SII`. Here is what worked eventually:

```
sudo apt-get update
sudo apt-get install -y build-essential libcups2-dev libcupsimage2-dev
```

Download the zip file from this repository:
	`https://github.com/gonutz/smart-label-printer-slp-linux-driver`

Extract the zip file, step into its `src` directory and run
```
make
sudo make install
```
The build has warnings but should succeed. If it does not, you might have to install additional dev packages on your distribution, the compile errors should point you in the right direction in that case. Please create a issue on the [Github page](https://github.com/gonutz/smart-label-printer-slp-linux-driver) for this repo in that case.

After installing the driver there will be problems with cups, indicated by an error message about insecure file rights. Run these commands to prevent this:
```
sudo chown -R root /usr/lib/cups/filter/sekoslp.rastertolabel
sudo chgrp -R root /usr/lib/cups/filter/sekoslp.rastertolabel
sudo chmod 755 /usr/lib/cups/filter/sekoslp.rastertolabel
```

To install the printer itself (the above is just the driver), connect it via USB, turn it on and go to the OS's printer menu.

Add a new printer and configure its properties (these might be called `Additional Printer Settings` under Ubuntu. Your printer should be selectable from the a list as a USB-connected printer.

Navigate to the `Settings` tab, find the entry `Make and Model` and click `Change`.

In the `Change Driver` dialog, select `Provide PPD File`

Select the PPD file `.../smart-label-printer-slp-linux-driver-master/src/siislp440.ppd`, located in your Downloads folder or wherever you downloaded this repository to.

Make sure to select the option `Use the new PPD (Postscript Printer Description) as is.` to overwrite the default settings.

Confirm and close the `Change Driver` dialog. The correct driver is now selected for your printer.

Still in the printer settings, go to the tab `Printer Options`. Change the `Media Size` to be `Multi-Purpose (SLP-MRL)` and click `Apply`.

Your printer is now installed. Reboot your system to be on the safe side.

If you have selected your SLP440 as the default printer, you can use the terminal program `lp` to print a label. Just run `lp`, type in your label text, add a line break and hit `Ctrl+D` to start printing. If you get any errors at this point, create an issue in my Github repo.

*The following is the original README from the forked driver's repository*

# Linux CUPS drivers for Smart Label Printers slp100 slp200 slp240 slp440 slp450 slp620 slp650

## This is a mirror

I originally found this driver as a ZIP file after going through several layers of dreadful web forms,
giving over personal details etc.

This is an unofficial, unmaintained mirror. The driver code is GPL.

The [initial commit](https://github.com/paulfurley/smart-label-printer-slp-linux-driver/commit/3bfaa74b584414b57dc02cb3835a690f091dac30) should exactly match the contents of that ZIP file.

The original ZIP file checksums below:

- filename: `SLP-600-400-LinuxCUPSDriver_01.zip`
- md5: `d1bef35acc6ca6d4c201a0048c7e6ef4`
- sha256: `d92fdf5ca7f40c99262c56eb724c2ffd454c981230a98391d16799cb2e98277d`

## Other versions of this driver

- [danieloneill/SeikoSLPLinuxDriver](https://github.com/danieloneill/SeikoSLPLinuxDriver)
- [hholzgra/sii_440_cups](hholzgra/sii_440_cups)

## Linux packages

- [Arch Linux](https://aur.archlinux.org/packages/sii-slp-cups-git/)

## Acknowledgements

Thumbs up @c-mauderer for improving the build script, and creating an Arch Linux package!
