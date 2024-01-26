# Where to get E-Sys
Leave a post in this thread and you should get a starter pack (download and installation guide and usage guides) from Shawn (the absolute pro in the space).
https://f80.bimmerpost.com/forums/showthread.php?t=1814782

# Where to get FSC codes
Send Shawn a PM and he will suggest contacts, I purchased OEM codes from Gerry <warpeddriveservices@gmail.com>

# How to import FSC code
See Shawn's guide, base64 conversion can be skipped in newer E-Sys now:  
https://www.bimmerfest.com/threads/fsc-activation-code.674597/?post_id=7378290&nested_view=1&sortby=oldest#post-7378290

Import FSC code first before VO and FDL code the functions.

# About PSdZ data
The differences between PSdZ lite and full is simply in psdzdata/swe folder. Within that folder:
- fafp subfolder is needed for diagnosis (so ISTA+ diagnosis only install will include this)
- In addition to fafp subfolder, cafd subfolder is needed for coding (VO and FDL coding)
- All other folders contain software/firmware, and are only needed for programming/flashing
- For coding, PSdZ data used by E-Sys must be >= (more recent than) the firmware/software (i-Step) version of car

# Backup on first connection
## Connection via E-Sys (this is included in the start pack guide)
- Select Target F056 with the more recent i-level and the one without DIRECT suffix
- Select connection via VIN in interface
- Under 'vehicle-specific parameter (optional)', select 'Series, I-Step shipment' (think that's the default), leave dropdown boxes BLANK. DO NOT select 'read parameters from VCM'
- Click 'Connect' button
## Backup
### FA (Vehicle Order)
Connect => Expert Mode (on the left panel) => Coding (on the left panel) => 
Vehicle Order (around the top in the right panel) => Press Read button to Read FA => Press Save to save to desired location
### SVT
Connect => Expert Mode => Coding => Go to Coding box and press 'Read (ECU)' (NOT 'Read (VCM)') => Save to desired location
### CAFD (FDL codes)
After ECU read, in the ECU list on the left, there are files with green dots. Select those files (one by one or hold control to select all of them), then Coding box/Read Coding Data.  
The codes will be loaded in C:\Data\CAF, back-up those files in desired location.  
This step is optional as they can be easily re-coded as long as you have original FA on hand.
### FSC
1. Get diag address of ECUs:
   Connect => Comfort Mode => FSC => Check FSC status => note down the diag addresses, for example HU is 99 (0x63) and KAFAS is 93 (0x5D)  
2. Get FSC code status:
   Then Expert Mode => enter hex code of ECU to backup in Diag Addr (for example enter 0x63) => click Identify => In action box lower down, select GetStatus and press >> => Click Start
3. Backup each FSC code:
   In same window, enter the App Num in hex (for example 0x9C for BMW App), enter Upgrade Index in hex and click 'Read'.
   Then 'Save' to save to a file, remember to put ECU name, diag addr, app num and index in the name, for example I saved as HU0x63_App0x9C_Idx0x01.fsc
4. Repeat Step 3 for each FSC code in the ECU
5. Repeat Step 2-4 for each ECU (ignore the ECUs without available FSCs, use information in Comfort Mode step 1)
6. You now have your own FSC recovery pack if needed

