---
title: Register devices in Microsoft Managed Desktop
description: Register devices so they can be managed by Microsoft Managed Desktop
ms.prod: w10
author: jaimeo
ms.author: jaimeo
ms.localizationpriority: medium
---

# Register devices in Microsoft Managed Desktop

[//]: # (This draft version topic focuses on the self-service option--how different is the partner thing?)
>Note: This article describes the steps for you to register devices on your own. The process for Partners is documented [Somewhere else LINK].

Managed Desktop can work with brand-new devices or you re-use devices you might already have (which will re-image them). You can register devices by using the Managed Desktop Admin Center or save time and gain flexibility by using an API.

## Prepare to register devices

If you haven't already obtained the devices you want to use, see [Order devices LINK] for options to get some. In either case, you should check {device list LINK} to ensure that they meet the appropriate system requirements.

Whether you're working with completely new devices or re-using existing ones, to register them with Microsoft Managed Desktop, you'll need to prepare a comma-delimited (CSV) file. This file should include the following information for each device:

>Note: This format is for self-service only. Partners have a different format documented [Somewhere else LINK].

[//]: # (I see a mention in the Word outline of this CSV file being different depending on the "actor"--can you clarify?)
[//]: # (Pretty different - the fields for Manufacturer, Model, and Serial have to be EXACT matches, and Hardware Hash is not required at all.)

These values are used for display purposes, and don't need to match properties from the device exactly.
- Device manufacturer (example: Microsoft) 
- Device model (example: Surface Laptop)
- Device serial number

The Hardware hash must be an exact match.
- Hardware hash

To obtain the hardware hash you can ask for help from your OEM or Partner, or follow these steps for each device:

1. Open an admin PowerShell window
2. Install-Script -Name Get-WindowsAutoPilotInfo
3. Set-ExecutionPolicy -ExecutionPolicy Unrestricted
4. .\Get-WindowsAutoPilotInfo -OutputFile <path>\hardwarehash.csv

These steps can also be done on a brand new device before going through OOBE for the first time by following these steps:

1. On a different device, insert a USB drive
2. Launch an admin PowerShell window
3. Save-Script -Name Get-WindowsAutoPilotInfo -Path <pathToUsb>
4. Turn on the target device, and do not progress OOBE.
    >Failure to do so will result in needing to reset or reimage the device.
5. Insert the USB
6. Hit Shift+F10 
7. type powershell
8. cd <pathToUsb>
9. Set-ExecutionPolicy -ExecutionPolicy Unrestricted
10. .\Get-WindowsAutoPilotInfo -OutputFile <path>\hardwarehash.csv
11. shutdown -s -t 0

>Do not power on the target device again until Registration is complete. 

>[!NOTE]
>For your convenience, you can download a template for this CSV file from {deeplink into product}.

Your file needs to include the **exact same column headings** as the sample one (Manufacturer, Model, etc.), but your own data for the other rows. If you use the template, open it in a text editing tool such as Notepad, and consider leaving all the data in row 1 alone, only entering data in rows 2 and below. 
    
  ```
 Manufacturer,Model,Serial Number,Hardware Hash
  Microsoft Corporation,Surface Laptop,016520771357,eidkaofjeioaofjfoieoajfoiejofiaoiojeifjojew
  
  
  ```
>Failure to change any the sample data will result in your registration being rejected.   

[//]: # (do the devices themselves need any kind of prep or settings? Firewall things? Diag data turned on? Or do they only need to be powered on and on the network? Any other network settings, domain, NAT, or whatever?)
[//]: # (Nope! We make it easy.)

## Register devices by by using the Admin Center

From the MMD Admin Portal [aka.ms/mmdportal LINK], select Devices in the left navigation. Select **+ Register devices**; the fly-in opens:

{screenshot to reassure user they're in the right place}

>[!IMPORTANT]
[//]: # (Sadly this isn't true. We can remove this note - but leaving it now until we have a chance to chat about it.)
>Registering any existing devices with Managed Desktop will completely re-image them; make sure you've backed up any important data prior to starting the registration process.


Follow these steps:

1. In **File upload**, provide a path to the CSV file you created previously.
2. Optionally, you can add an **Order ID** or **Purchase ID** for your own tracking purposes. There are no format requirements for these values. {possibly mention where one can view/sort/search those later? Autopilot uses them.}
3. Select **Register devices**.
4. The system will add the devices to your list of devices on the Devices blade, marked as Registration Pending.
5. Registration typically takes less than 10 minutes, and when successful the device will show as "Setup needed" meaning it's ready and waiting for an end-user to start using.

[//]: # (Can we offer any kind of estimate of how long to expect this to take?)
[//]: # (I Like this idea. Added something above.)

You can monitor the progress of device registration on the main **Microsoft Managed Desktop - Devices** page. Possible states reported there include:

[//]: # (possible screenshot highlighting where the status is shown)
[//]: # (maybe better as a table so we can explain what the states mean)

- Registration pending
- Registration Failed (sub-list below)
- Setup Needed
- Active
- Inactive


## Register devices by using an API

{what it's good for - more for repeatability/flexibility rather than # of devices} - This is a great way of phrasing it. It helps if you have to complete many separate registrations, but if you just do one big batch of 10,000, the UI is just as good. It's really meant for automation and Partners than for Customers. 

{what language or framework or whatever is it?} - It's a REST API, and we have a C# sample app to help them get started. 

{where to obtain} - We decided to self-publish as an Alpha release. This should read something like "ask for help from your microsoft contract to get added to the preview.". 



## Troubleshooting

[//]: # (kinda depends on how much ends up being needed here. If very little, could go in the same section as table listing progress/outcomes. If more, its own section here. If a lot more, we can make a separate topic for it)
[//]: # (I think the table above covers this pretty soundly.)

## Partner Process




