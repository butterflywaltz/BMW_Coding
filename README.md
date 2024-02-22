# BMW_Coding
**Programming and coding has risks, read as much as possible to be aware of possible risks and outcomes and proceed at own risk.**

## Background
Recently bought a MINI Countryman F60 LCI registered end 2020. There is a big software update in Mar 2021 for that model, so most of the LCI models are getting the new software, early built ones missed out. Trying to retrofit that and also wants to retrofit Driving Assistant (pedestrian recognition and traffic sign info)

## About MINI Driving Assistant Pack
Driving Assistant Pack includes 2 options:
- Option 5AS: Driving Assistant (does not need SAS ECU)
- Option 541: Active Cruise Control (need SAS ECU)

Option 5AS Driving Assistant consists of:
- Highbeam Assist (I have option 552, so this is included)
- Front Collision Warning / Front Vehicle Identification (this is included in 5AV Active Guard, BE FSC code not needed)
- Pedestrian Identification (BF FSC code needed)
- Lane Departure Warning (this is added with software later than and including 21-03-xxx, no FSC code needed)
- Speed Limit Information (7E FSC code needed)
- This option does not need SAS module (Optional Extra System Module)

Option 541 ACC in MINI is a camera based system, and does need SAS module, one easy way to check if SAS module is present is to check if fuse 60 is present in the fusebox. I do not have SAS and have no plan to retrofit it, so main goal is to enable 5AS.  
https://fuse-box.info/mini/mini-countryman-f60-2017-2022-fuses

## What is needed
- Need a Windows 10 laptop (this was done by upgrading an old macbookpro)
- Need ISTA+ for programming (don't know how to use e-sys to program), this will take about 380G of disk spaces, so definitely go for 1T SSD drives [Free software]
- Need e-sys for VO and FDL coding [Free software]
- Need BimmerUtility for FDL editing [Payment needed]
- Need FSC codes (7E and BF) [Payment needed as these are OEM codes]
- Need a Ethernet to OBD2 cable

## Some observations post project
- Lane Departure Warning is working but warning is only delivered through vibration, so if steering wheel has no vibration (if no heating, no vibration) then you won't get any warning...it's pretty stupid for BMW to do this...trying to figure out a way but so far no success
- Changing from iDrive 5 to iDrive 6 Lite also means that some event view of center ring loses function, I quite like them, again pretty stupid to delete them from system, especially iDrive 6 is just a skin change:
  - Auto Start/Stop
  - Driving mode
  - Menu selection
- Sports Display: cannot figure out how to get it work only in M/S gear, even though check box is coded and shown and selected

## More about Lane Departure Warning (LDW)
3 prerequisites as far as I know:
1) you need later than 21-03-xxx i-level, which you can check via the user profile export to USB
2) you need a vibration steering wheel to receive warning, if your wheel has heating, then it likely has vibration, otherwise...likely no luck
3) you need a digital KOMBI (digital cockpit, mine is option 6WB) for the status icon to show

Check this post for the coding needed for BDC, KAFAS, HU
https://www.minif56.com/threads/lane-change-warning-and-lane-departure-warning-on-mini-cooper-2020.92391/

For KOMBI, do below:
TLC_VERBAUT: nicht_aktiv (00) -> aktiv (01)
ST_TLC_TIMEOUT: nicht_aktiv (00) -> aktiv (01)
ST_TLC_ALIVE: nicht_aktiv (00) -> aktiv (01)
ST_TLC_APPL: nicht_aktiv (00) -> aktiv (01)

But even if the whole coding is successfully applied, and system is up and running, if you don't have a vibration wheel, you won't get any type of warning...I digged a little bit into the coding file today, basically under TLC section (TLC is german for LDW I guess) in KAFAS, it can send 3 messages:
1) message 327 is sent when KAFAS detects two lanes and informs BDC that the system is working. BDC then tells KOMBI to show a green LDW status icon, showing the system is online
2) message 18A is sent when you close to one lane, BDC then tells the wheel to vibrate
3) message 345 can be configured to be sent when you close to one lane, in old systems (which I read in very old post of BMW owners, not MINI), BDC will tell KOMBI to show warning visually using 345

But looking at ComAdapterPdu section of BDC, the treatment of message 345 is just not implemented. So currently only way to receive warning is via a steering wheel vibration. A dumb implementation choice of BMW.
