ww
https://www.reddit.com/r/archlinux/comments/14akrgg/how_to_get_kvm_libvirtd_and_virtmanager_working/?rdt=60671

https://www.fosslinux.com/2484/how-to-install-virtual-machine-manager-kvm-in-manjaro-and-arch-linux.htm

GPU Pass:
To pass through your Intel GPU to a Windows VM on Arch Linux, you will need to use VFIO (Virtual Function I/O) along with QEMU/KVM to manage your virtual machine. Here's a step-by-step guide to achieve this:

### Prerequisites:

1. **Intel CPU with VT-d support**: Make sure that your CPU and motherboard support Intel's VT-d (Intel Virtualization Technology for Directed I/O).
2. **IOMMU support**: Your Linux kernel should support IOMMU (Input-Output Memory Management Unit). On Intel systems, this is typically enabled via `intel_iommu=on`.
3. **QEMU, KVM, and VFIO modules installed**: You will need to install a few packages and configure your system.

### Step 1: Enable Intel VT-d (IOMMU) in BIOS/UEFI

- Reboot your machine and enter the BIOS/UEFI settings.
- Enable **Intel VT-d** (Virtualization Technology for Directed I/O) under the CPU configuration or similar section.

### Step 2: Enable IOMMU in the Kernel

1. Edit the bootloader configuration file to enable IOMMU support.
    
    For **GRUB**, modify the `/etc/default/grub` file:
    
    ```bash
    sudo nano /etc/default/grub
    ```
    
    Add `intel_iommu=on` to the `GRUB_CMDLINE_LINUX_DEFAULT` line:
    
    ```bash
    GRUB_CMDLINE_LINUX_DEFAULT="quiet intel_iommu=on"
    ```
    
    Then, regenerate the GRUB configuration:
    
    ```bash
    sudo grub-mkconfig -o /boot/grub/grub.cfg
    ```
    
2. **Reboot** the system.
    

### Step 3: Install Necessary Packages

Install the following packages on Arch Linux:

```bash
sudo pacman -Syu qemu libvirt virt-manager vhost-vfio
```

You'll need `qemu`, `libvirt`, `virt-manager`, and `vhost-vfio` (for device passthrough). `virt-manager` is optional but helpful for managing the VM graphically.

### Step 4: Configure VFIO for GPU Passthrough

1. **Identify your GPU and associated devices**: You can find your GPU device IDs by running:
    
    ```bash
    lspci -nn
    ```
    
    Look for the Intel GPU in the output, and note the vendor and device IDs. For example, it might look like:
    
    ```
    00:02.0 VGA compatible controller [0300]: Intel Corporation UHD Graphics 620 (rev 07) [8086:5917]
    ```
    
    In this case, the vendor ID is `8086` and the device ID is `5917`.
    
2. **Bind the Intel GPU to VFIO**: To pass through the GPU, you need to bind it to the VFIO driver. Create or edit the file `/etc/modprobe.d/vfio.conf`:
    
    ```bash
    sudo nano /etc/modprobe.d/vfio.conf
    ```
    
    Add the following lines (replace `8086` and `5917` with your actual GPU's IDs):
    
    ```
    options vfio-pci ids=8086:5917
    ```
    
3. **Update initramfs** to ensure the VFIO driver is loaded at boot:
    
    ```bash
    sudo mkinitcpio -P
    ```
    

### Step 5: Create a Windows VM with GPU Passthrough

1. **Launch Virt-Manager** (if using `virt-manager` GUI):
    
    ```bash
    virt-manager
    ```
    
2. **Create a new VM**:
    
    - Select "Local install media (ISO)" and choose your Windows installation ISO.
    - During the VM setup process, make sure to set up the VM to use UEFI (required for GPU passthrough).
3. **Configure the GPU Passthrough**:
    
    - After the VM is created, right-click on the VM in Virt-Manager and select **Open**.
    - Shut down the VM if it’s running.
    - Go to **View > Details > Video** and add your Intel GPU as a PCI device under **Add Hardware**.
    - Select your Intel GPU from the list of available devices.
4. **Make sure to set the VM’s CPU to "host-passthrough" for optimal performance**:
    
    - In the VM settings, under **Processor**, set the CPU model to `host-passthrough`.
5. **Enable IOMMU in the VM configuration**:
    
    - Go to the **XML configuration** of the VM (right-click > **Open XML**).
    - Ensure that the IOMMU flags are enabled. Add the following:
    
    ```xml
    <cpu mode='host-passthrough'>
      <feature policy='require' name='vmx'/>
      <feature policy='require' name='svm'/>
    </cpu>
    ```
    

### Step 6: Install Windows and GPU Drivers

1. **Install Windows**: Start the VM with your Windows installation ISO and proceed with the installation of Windows.
    
2. **Install Intel drivers** for the GPU once Windows is installed.
    
    - You may need to install the Intel Graphics Driver after the VM boots into Windows to properly utilize the GPU.

### Step 7: Test GPU Passthrough

Once the installation is complete, you should be able to use the Intel GPU in the Windows VM, and it should show up as the primary display adapter within Windows.

You can check the **Device Manager** in Windows to verify the GPU installation.

### Troubleshooting Tips

- If the system is not booting or there are issues with VFIO binding, check the `dmesg` logs for error messages related to VFIO or IOMMU.
- Make sure your kernel supports VFIO, and check if your CPU's IOMMU (Intel VT-d) is active by checking `dmesg | grep -i iommu`.

This setup should allow you to pass through your Intel GPU to a Windows VM on Arch Linux!