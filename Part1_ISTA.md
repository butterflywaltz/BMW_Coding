# ISTA installation
## Where to get ISTA+
You should get it FREE. In this thread, there is a google drive link and there is torrent file for ISTA+ 4.39.20 stand-alone.
https://bmwi.bimmerpost.com/forums/showthread.php?t=2041580
Anyway the paid options barely offer you any support, so do get it freely

## Pre-requisites
These are original requriements stated in the download:
1. Windows 10 v1903 or higher
2. Microsoft .NET Framework 4.8.x
3. Visual C++ Runtime 2015-2019 (Comment: not sure x64 or x86, for safty purpose, install both)
4. Google Chrome (Comment: not sure why)
5. Windows username should not contain spaces, dashes and special characters like "ö".

## How to install
1. After download, disconnect internet (not sure if needed but just in case)
2. First run the ista_standalone_installer, install to default paths is SUGGESTED (which is C:/EC-APPS/ISTA)
3. You may find SDP (programming data is missing), this is normal, do not attempt to change anything to fit SDP.7z before running installer
4. After installation, go to C:/EC-APPS/ISTA/PSdZ and create a folder named data_swi
5. Unzip vX.XX.XX_PSdZData.7z in the downloaded content to data_swi, after unzip, it should show psdzdata folder under data_swi
6. Go to downloaded folder, and go to files_only_for_manual_install/regfiles_full_sdp_blp and double click the registry file to import the registry keys (you may open it by notepad to study the content, basically it changes the psdzdata path to data_swi and put two programming related keys to True)
7. If PSdZData version is newer or older than app version, go to download folder and under files_only_for_manual_install, double click AppCheckDisable_to_use_different_DBs.reg to import keys to enable mismatched app and database version
8. The installation is complete and it should take about 380G spaces

## Prepare to program vehicle
1. Read the ISTA help file FIRST
2. On laptop
   - disable firewall and real-time protection
   - disable all battery saving measures, make sure laptop doesn't fall asleep
   - disconnect internet, ethernet is the only networking needed
3. On vehicle
   - Make sure vehicle is parked correctly (and electric parking brake applied)
   - export user profile
   - disable child-lock if that was enabled
   - disconnect any additional devices (for example, dashcam) to save power drain
   - remove all connected phones in vehicle (or alternatively, place relevant phones in air-safe mode)
   - make sure front passenger seat has no load
   - Open bonnet and connect battery charger to positive and ground access points, and make sure battery has at least 50% (just a safe measure)

## Connecting to vehicle and start programming
1. Turn on ignition
2. Click driver belt and leave driver door physically open
3. Connect enet cable to the car
4. Open ISTA
5. In ISTA, Operations tab/New/Read-out vehicle data and click on Complete identification, background processes will run and in the end, ECU trees will show
6. Go to Vehicle management/Software update/Comfort tab, it will calculate the service plan, after calculation, click on display service plan to see the contents
7. Then click on execute service plan, and the programming (flashing ECUs process) will start, the vehicle system will shutdown, and a bootloader will appear to show status of data transfer, status is also shown in ISTA on laptop
8. The system will restart after data transition and encodings are complete, let ISTA finish the residual processes
9. Whan asked to close door, route laptop power cable under the driver door and sit on driver seat and close driver door. This is for door and window initialisations
10. Some initialisation process may fail and many error will come in the vehicle, DO NOT PANIC YET, let ISTA finish
11. After ISTA has finished, if it says there are residual plans and errors, terminate the operation for now. After that disconnect cable and turn off ignition. Go for some coffee and food, leave for at least 15min
12. Come back and turn ignition on, and connect cable and ISTA and continue the residual plan. Most of errors should be gone, even some left, seems not impacting usage at all
13. When you see no major errors, especially in your vehicle screen, disconnect everything properly and go for a test drive

## Difference post upgrade
(Upgraded from F056-20-07-545 to F056-22-11-530)
- Transmission seems smoother
- No other changes for now, but I need a version later than 21-03 for various purposes
