
Tried to connect the headphone using bluetooth and it wasn't working. In settings, I could enable bluetooth but nothing was happening and there was no pair button to pair a device although it instructed me to press the pair button.

installed sudo pacman -S bluez bluez-utils
Usually after this step, if bluetooth service is enaabled, bluetoothctl should list down the devices or do something- which wasn't happening in my case because the service itself wasn't started. 

installed usbutils (for lsusb to work)

```
sudo systemctl enable bluetooth.service
sudo systemctl start bluetooth.service
```

