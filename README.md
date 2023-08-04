*1)* Pair ALL bluetooth devices in linux (it is to have the files you will need to edit later)

*2)* Pair ALL bluetooth devices in Windows 10. If you know how, get the `MAC address` id from your bluethooth keyboard, we will need it later

*3)* Reboot and go back to Linux

*4)* Install `chntpw` package, this is needed to read the registry keys from Wintendo
```
sudo apt-get install chntpw
```

*5)* Mount your Wintendo system drive in Linux and enter to this folder
```
cd /[MountedDrive]/Windows/System32/config
```

*5)* Execute this command inside that folder
```
chntpw -e SYSTEM
```

*6)* In the `chntpw` raised console, enter in the bluetooth registry keys list like this:
```
cd \ControlSet001\Services\BTHPORT\Parameters\Keys
```

*7)* run `ls` command to show you a `Unique id` lists
```
(...)\Services\BTHPORT\Parameters\Keys> ls
Node has 1 subkeys and 0 values
  key name
  <00f48d9e41aa>
```
Like this one equivalent to `00:F4:8D:9E:41:AA` 

*8)* Enter in each `Unique id` folder to search for your MAC address of your bluetooth device in each `value name`
```
(...)\Services\BTHPORT\Parameters\Keys> cd 00f48d9e41aa
(...)\BTHPORT\Parameters\Keys\00f48d9e41aa> ls
Node has 0 subkeys and 3 values
  size     type              value name             [value if type DWORD]
    16  3 REG_BINARY         <34885dd82480> <- This is our keyboard
    16  3 REG_BINARY         <34885db5f481>
```

*9)* Get the hex code (that we will call friendly: the `pairing key`) from the keyboard `MAC address` value
```
(...)\BTHPORT\Parameters\Keys\00f48d9e41aa> hex 34885dd82480
Value <34885dd82480> of type REG_BINARY (3), data length 16 [0x10]
:00000  BE 7F B1 99 23 29 D5 B2 6A E2 F6 96 2E FD 16 8A ....#)..j.......
```

*10)* So... we will copy the `pairing key` that use Wintendo to pair your device: `BE7FB1992329D5B26AE2F6962EFD168A`
We will use this `pairing key` to make Linux pair our keyboard, without re-pairing again. We will use now de `Unique Id`, the `MAC Address` and edit this file in your Linux drive:

`sudo nano /var/lib/bluetooth/[Unique ID]/[Mac Address]/info`
```
sudo nano /var/lib/bluetooth/00\:F4\:8D\:9E\:41\:AA/34\:88\:5d\:d8\:24\:80/info
```

*11)* In the `info` file go to the `[LinkKey]` section and replace the `Key` value with the `pairing key`
```
Key=BE7FB1992329D5B26AE2F6962EFD168A
```

*12)* Save and restart the bluetooth service in Linux and you are ready to go
```
sudo service bluetooth restart
```
