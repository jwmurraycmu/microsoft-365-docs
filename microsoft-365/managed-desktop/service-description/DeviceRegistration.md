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


>[!NOTE]
>This topic focuses describes the steps for you to register devices on your own. If you're working with a Partner, see [Somewhere else LINK].

Managed Desktop can work with brand-new devices or you re-use devices you might already have (which will re-image them). You can register devices by using the Managed Desktop Admin Center or save time and gain flexibility by using an API.


## Prepare to register devices

If you haven't already obtained the devices you want to use, see [Order devices LINK] for options to get some. In either case, you should check {device list LINK} to ensure that they meet the appropriate system requirements.

Whether you're working with completely new devices or re-using existing ones, to register them with Microsoft Managed Desktop, you'll need to prepare a comma-delimited (CSV) file. This file should include the following information for each device:

>[!NOTE]
>You should use this format only if you are registering the devices yourself. If you're working with a Partner, see [Somewhere else LINK].

[//]: # (I see a mention in the Word outline of this CSV file being different depending on the "actor"--can you clarify?)
[//]: # (Pretty different - the fields for Manufacturer, Model, and Serial have to be EXACT matches, and Hardware Hash is not required at all.)

These values are used for display purposes, and don't need to match properties from the device exactly. For example, it doesn't matter if you list a device model as "Surface" or "Surface Laptop."

- Device manufacturer (example: Microsoft) 
- Device model (example: Surface Laptop)
- Device serial number
- Hardware hash

>[!NOTE]
>Unlike the other values, the hardware hash must be an exact match.

To obtain the hardware hash, ask for help from your OEM or Partner, or follow these steps for each device:

1. Start a PowerShell prompt with administrator privileges.
2. Run `Install-Script -Name Get-WindowsAutoPilotInfo`
3.Run `Set-ExecutionPolicy -ExecutionPolicy Unrestricted`
4. Run `.\Get-WindowsAutoPilotInfo -OutputFile <path>\hardwarehash.csv`

{and then copy the hardware hash values out of this output .csv into the main one that also has the devices names and stuff, or what?}

>[!NOTE]
>For your convenience, you can download a template for this CSV file from {deeplink into product}.

Your file needs to include the **exact same column headings** as the sample one (Manufacturer, Model, etc.), but your own data for the other rows. If you use the template, open it in a text editing tool such as Notepad, and consider leaving all the data in row 1 alone, only entering data in rows 2 and below. 
    
  ```
 Manufacturer,Model,Serial Number,Hardware Hash
  Microsoft Corporation,Surface Laptop,016520771357,eidkaofjeioaofjfoieoajfoiejofiaoiojeifjojew
  
  
  ```
>Failure to change any the sample data will result in your registration being rejected.   

>[!TIP
>If you're working with a brand new device, you can obtain the hardware hash without going through the whole out-of-box experience by following these steps:

1. On a different device, insert a USB drive.
2. Start a PowerShell prompt with administrator privileges.
3. Run `Save-Script -Name Get-WindowsAutoPilotInfo -Path <pathToUsbDrive>`
4. Turn on the target device, but do not go through the out-of-box experience. If you do proceed with the experience, you'll have to reset or re-image the device to register it later.
5. Insert the USB drive, and then press SHIFT + F10.
7. Enter *powershell* to start a PowerShell prompt.
8. Run `cd <pathToUsbDrive>`
9. Run `Set-ExecutionPolicy -ExecutionPolicy Unrestricted`
10. Run `.\Get-WindowsAutoPilotInfo -OutputFile <path>\hardwarehash.csv`
11. Run `shutdown -s -t 0`

Do not power on the target device again until registration in MMD is complete.

{so MMD somehow knows where to grab that hardwarehash.csv file? Or do you have to somehow transfer that info in the main .csv file that has the other stuff like device name?}



[//]: # (do the devices themselves need any kind of prep or settings? Firewall things? Diag data turned on? Or do they only need to be powered on and on the network? Any other network settings, domain, NAT, or whatever?)
[//]: # (Nope! We make it easy.)

## Register devices by using the Admin Center

From the MMD Admin Portal [aka.ms/mmdportal LINK], select **Devices** in the left navigation pane. Select **+ Register devices**; the fly-in opens:

{screenshot to reassure user they're in the right place}

>[!IMPORTANT]

[//]: # (Sadly this isn't true. We can remove this note - but leaving it now until we have a chance to chat about it.)

>Registering any existing devices with Managed Desktop will completely re-image them; make sure you've backed up any important data prior to starting the registration process.


Follow these steps:

1. In **File upload**, provide a path to the CSV file you created previously.
2. Optionally, you can add an **Order ID** or **Purchase ID** for your own tracking purposes. There are no format requirements for these values. {possibly mention where one can view/sort/search those later? Autopilot uses them.} {Let's stay silent on this for now.}
3. Select **Register devices**.
4. The system will add the devices to your list of devices on the Devices blade, marked as Registration Pending.
5. Registration typically takes less than 10 minutes, and when successful the device will appear labeled **Setup needed** meaning it's ready and waiting for an end-user to sign in for the first time.

[//]: # (Can we offer any kind of estimate of how long to expect this to take?)
[//]: # (I Like this idea. Added something above.)
[//]: # (depends on the # of devices?)

You can monitor the progress of device registration on the main **Microsoft Managed Desktop - Devices** page. Possible states reported there include:

[//]: # (possible screenshot highlighting where the status is shown)
[//]: # (maybe better as a table so we can explain what the states mean)

- Registration pending
- Registration Failed: Export to see the full list of errors.
- Setup Needed
- Active
- Inactive


## Register devices by using an API

[//]: # (what it's good for - more for repeatability/flexibility rather than # of devices} - This is a great way of phrasing it. It helps if you have to complete many separate registrations, but if you just do one big batch of 10,000, the UI is just as good. It's really meant for automation and Partners than for Customers.) 
[//]: # (what language or framework or whatever is it? - It's a REST API, and we have a C# sample app to help them get started.) 
[//]: # (where to obtain - We decided to self-publish as an Alpha release. This should read something like "ask for help from your microsoft contract to get added to the preview.")

If device registration is something you anticipate having to do frequently or automate, especially with different variables each time, you can save effort by using a REST API. To take advantage of this API, check with your Microsoft contact to join a preview.



## Troubleshooting

[//]: # (kinda depends on how much ends up being needed here. If very little, could go in the same section as table listing progress/outcomes. If more, its own section here. If a lot more, we can make a separate topic for it)

[//]: # (James and Jose are working on changing the Error Codes to be better.)

| Error | Details |
|----------------------|---------------------|
| Unexpected Error |Your request could not be automatically processed. Contact Support [link to Ops page] and provide the Request ID from your error export. |
| Invalid Hardware Hash | The hardware hash you provided for this device was not formatted correctly. Double-check the hardware hash (see [Prepare to register devices](#prepare-to-register-devices)) and then resubmit. |
|Device already registered |This device is already registered to your company. No further action required. |
| Device not found |We couldn’t de-register this device because it does not exist in your company. No further action required. |
| Device claimed by another company | This device has already been claimed by another company. Check with your device supplier. |
| Device not found | We couldn’t register this device because we could not find a match for the provided manufacturer, model, or serial number. Confirm these values with your device supplier. |

[//]: # (A few notes/questions: we can't use HTML tables. We don't say "please" except in very particular circumstances. Would it be possible to change "Invalid hardware hash" to "Hardware hash not valid"? [we avoid "invalid"]. Earlier we heavily implied that the man./model didn't have to match exactly, but here it seems it'll throw an error if not...so what's the story there? Is "de-register" a typo--i.e., should it be "register?" Or is there some kind of de-registration action that we just haven't discussed yet? Oh, do we explain anywhere how to export errors, since we ask for that in the first row of the table?)


[//]: # (I'll break out this partner stuff into its own topic as soon as I figure out how to do that through this editing method--I haven't edited anything below this point just yet either.)
## Partner Process

### Preparing for Registration 
Before completing registration for a customer, you must first establish a relationship with them on Partner Center [Partner Center LINK].

To complete registration for your customer, you must create a CSV file.
>Note: This format is for Partners only. Customers have a different format for self-service documented [Somewhere else LINK].

>[!Important]These values must match the manufacturer values from SMBIOS exactly
- Device manufacturer (example: Microsoft) 
- Device model (example: Surface Laptop)
- Device serial number

[//]: # (hang on--how come the values have to match exactly here, but it doesn't matter with the self-service process??)

## Register devices by by using the Admin Center
Registering by the UI is the same as for self-service, except getting to the portal is slightly different. Instead of using aka.ms/mmdportal, they must instead:
1. Navigate to PArtner Center
2. Click Customers
3. Click the customer they want to manage
4. Click Service Administration
5. Click Intune
6. Search "mmd" in the top search box of the Azure portal
7. click Devices and it's the same from here.

## Register devices by using an API
Registering by API is the same as self-service, except that the Hardware Hash property of the Device collection is optional as described in the CSV section. 
