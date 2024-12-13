 

Create a custom ESXi 7.0 Images including the USB NIC Driver 

      Download USB NIC Fling( https://flings.vmware.com/usb-network-native-driver-for-esxi ) (ESXi700-VMKUSB-NIC-FLING-34491022-component-15873236.zip) 
      Copy the USB NIC Driver to your build directory (eg. c:\esx\ 
      Open PowerShell 
      (optional) Install VMware PowerCLI from the PowerShell GalleryInstall-Module -Name VMware.PowerCLI -Scope CurrentUser 
      Change to your build directorycd c:\esx\ 
      Clone the original Image Profile, add the driver and export to ISO and Zip Bundle. 
      Add-EsxSoftwareDepot https://hostupdate.vmware.com/software/VUM/PRODUCTION/main/vmw-depot-index.xml 
      Get-EsxImageProfile -Name "ESXi-8*" 
      Export-ESXImageProfile -ImageProfile "ESXi-7.0.0-15843807-standard" -ExportToBundle -filepath ESXi-7.0.0-15843807-standard.zip 
      Remove-EsxSoftwareDepot https://hostupdate.vmware.com/software/VUM/PRODUCTION/main/vmw-depot-index.xml 
      Add-EsxSoftwareDepot .\ESXi-7.0.0-15843807-standard.zip 
      Add-EsxSoftwareDepot .\ESXi700-VMKUSB-NIC-FLING-34491022-component-15873236.zip 
      New-EsxImageProfile -CloneProfile "ESXi-7.0.0-15843807-standard" -name "ESXi-7.0.0-15843807-USBNIC" -Vendor "virten.net" 
      Add-EsxSoftwarePackage -ImageProfile "ESXi-7.0.0-15843807-USBNIC" -SoftwarePackage "vmkusb-nic-fling" 
      Export-ESXImageProfile -ImageProfile "ESXi-7.0.0-15843807-USBNIC" -ExportToIso -filepath ESXi-7.0.0-15843807-USBNIC.iso 
      Export-ESXImageProfile -ImageProfile "ESXi-7.0.0-15843807-USBNIC" -ExportToBundle -filepath ESXi-7.0.0-15843807-USBNIC.zip 

  You should now have two files in your build directory: 
  - ESXi-7.0.0-15843807-USBNIC.iso     
  - ESXi-7.0.0-15843807-USBNIC.zip 

With the two files, you can either make a fresh ESXi 7.0 install or upgrade from previous versions. 

Create a Bootable ESXi 7.0 Installer USB Flash Drive 

    Download Rufus 
    Connect the USB flash drive to your computer 
    Open Rufus 
    Select your Device 
    Select your custom ISO file as Boot selection 
    Do not change anything else 
  
Rufus 39.1624 (Portable) Drive Properties Device NO_LABEL [32 GB] Boot selection ESXi-7.O.O-15843807-USBNlC.iso Persistent partition size Partition scheme CPT v Show advanced drive properties Format Options Volume label ESXl-7.OO-15843807-USBNlC File system FAT32 (Default) v Show advanced format options x SELECT 0 (No persistence) Target system UEFI (non CSM) Cluster size 16 kilobytes (Default) Status 00 1 device found READY START CLOSE
![rufus](https://github.com/user-attachments/assets/648031d6-6513-48ec-8aa2-67a9f1623428)
    
    Press START 
    Use the Flash Drive to install ESXi 7.0. 

 In some cases, ESXi Installation might fail at 81%. See [here](https://www.virten.net/2020/07/solution-esxi-installation-with-usb-nic-only-fails-at-81/) for a solution. 

Upgrade to ESXi 7.0 with USB NIC Driver 

Updating previous versions is very simple. Copy the zip bundle to a datastore and run the following command and reboot the host: 

> esxcli software profile install -p ESXi-7.0.0-15843807-USBNIC -d /vmfs/volumes/[datastore]/ESXi-7.0.0-15843807-USBNIC.zip 
> esxcli software profile install -p ESXi-7.0U3e-19898904-addnic -d /vmfs/volumes/datastore1/ESXi-7.0U3e-19898904-addnic.zip 
