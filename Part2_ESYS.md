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
6. You now have your own FSC back-ups (note this may not be the recovery pack, I noticed that even if FSCCertStatus showing unavailable, the FSC are still imported using a certificate, so you don't have certficiate backup)

## Procedure
### Import FSC
Received FSC file should be like FSC_YOURVIN_ABCDEFGH.fsc with a .der certificate. ABCD is the App number in hex, input like 0xABCD, and EFGH is the Upgrade Index in hex, input like 0xEFGH, in fact load a FSC file will input those App number and Upgrade Index automatically.    
Repeat for all FSC codes:  
Connect => Settings/FSC, select the right FSC certificate => Comfort Mode => FSC => Click 'Read FA (VCM)' => Input Diagnosic Address (KAFAS is 0x5D) => Identify => Browse your FSC file to load it, App Num and UpgIdx auto-filled => Click 'Upgrade FSC' to import and activate => 'Check FSC Status' to verify

### VO Code 5AV -> 5AS (Retrofit of Driving Assistant)
1. Edit and Validate FA  
   Connect => Expert Mode => Coding => Vehicle Order box / Read => Save as FA_Upgrade_5AV_to_5AS_WRITE => click 'Edit'
   In editor, expand to SALAPA Element => Change 5AV to 5AS in bottom window => Click on save button (top right of bottom window) => Right-click FA and select 'Calculate FP' to validate FA => Save button (top bar, disk button)
2. Write FA to Vehicle  
   Expert Mode => VCM => In the file tab, load saved FA file => Right-click FA and select 'Calculate FP' to activate => Go to Master tab next to it => Click 'Write FA FP'
3. VO Code ECUs  
   Expert Mode => Coding => Vehicle Order box / Read (now verify it's the new FA with 5AS read-out from vehicle) => On the right, click 'Read (ECU)' => Select each of HU, KAFAS, KOMBI and BDC, right click and select 'Code' (do it one-by-one, you should see no errors)
4. Back-up coding
   Select each of HU, KAFAS, KOMBI and BDC, right click and select 'Read Coding Data', back up the ncd files in C:/Data/CAF

### VO Code Zeitkriterium to 0321 (Lane Departure Warning and new system look)  
VO date code is more tricky, not in terms of progress, but in terms of effect. So one may use it to explore the FDL codes related to the change and then VO code back and apply selected FDLs. If your Zeitkriterium is later than 0321, skip this whole section.  
1. Edit and Validate FA  
   Same as previous, except this time change Zeiktriterium to 0321 (note, it is in mmyy format and not all dates are valid, usually, try Mar, Jul, Oct, Nov, usually goes with i-level date). Calculate FP and save as FA_VODate_2020_to_2021_NOWRITE
2. DO NOT write this FA to vehicle, go to Coding, the modified FA will automatically be loaded (a prompt will ask if to do so)
3. VO Code ECUs: HU, KAFAS, KOMBI and BDC
4. Back-up coding

### Corresponding FDL codes for the above VO codings
|ECU\FDL Changes   | SALAPA 5AV->5AS | VO Date 2020 -> 2021  |
|---|---|---|
| HU  | Related to Active Cruise Control (not need, effect to be observed): <br>ACC_SGN: nicht_aktiv (00) -> aktive (01)<br>Pedestrian Alert: <br>PERSONENWARNUNG_TAGS: nicht_aktiv (00) -> gen_1 (01)<br>PERSONEN_WARNUNG: nicht_aktiv (00) -> aktiv (01) | New UI:<br> HMI_ID_VERSION: id5_plus_plus (02) -> id6_light (03)<br>Lane Departure Warning:<br> SPURVERLASSENSWARNUNG: nicht_aktiv (00) -> gen_1 (01)|
| KOMBI  | Speed Limit Information:<br>SPEED_LIMIT: nicht_aktiv (00) -> aktive (01)<br>HUD_SLI_WARNUNG_ENABLE: nicht_aktiv (00) -> aktive (01)<br>RCOG_TRSG_TIMEOUT: nicht_aktiv (00) -> aktive (01)<br>RCOG_TRSG_APPL: nicht_aktiv (00) -> aktive (01)<br>No-Pass Info:<br>ANZEIGE_NPI: nicht_aktiv (00) -> aktive (01)<br>Night Vision:<br>FGS_NIVI_KI_ENABLE: nicht_aktiv (00) -> aktive (01)<br>NIVI_ENABLE: nicht_aktiv (00) -> aktiv (01)<br>HUD_NIVI_ENABLE: nicht_aktiv (00) -> aktive (01)<br>High Beam Assist (effect to be seen):<br> ST_MAB_ASST_TIMEOUT: nicht_aktiv (00) -> aktive (01)<br>ST_MAB_ASST_APPL: nicht_aktiv (00) -> aktive (01)<br> Unknown:<br>STAT_OBJ_COOR_TIMEOUT: nicht_aktiv (00) -> aktive (01)<br>STAT_OBJ_COOR_APPL: nicht_aktiv (00) -> aktive (01)<br> | Seat Belt (does not seem right, to be reverted):<br> GURT_STATUS_BLINKEN: nicht_aktiv (00) -> aktiv (01)<br>GURT_FOND_STATUS: 3_sitze (02) -> generation_2 (04)<br>New UI:<br>FARBSET_NEU: alt (00) -> neu (01)<br>NAVICOLOR_TABLE: farbwelt_id5 (02) -> farbtabelle_2103 (03)<br>Lane Departure Warning:<br>TLC_VERBAUT: nicht_aktiv (00) -> aktiv (01)<br>ST_TLC_TIMEOUT: nicht_aktiv (00) -> aktiv (01)<br>ST_TLC_ALIVE: nicht_aktiv (00) -> aktiv (01)<br>ST_TLC_APPL: nicht_aktiv (00) -> aktiv (01)<br>Unknown (possibly LDW related):<br>ST_DISP_BLTW_TIMEOUT: nicht_aktiv (00) -> aktiv (01)<br>ST_DISP_BLTW_APPL: nicht_aktiv (00) -> aktiv (01)<br>ST_CS_TIMEOUT: nicht_aktiv (00) -> aktiv (01)<br>ST_CS_APPL: nicht_aktiv (00) -> aktiv (01)<br>|
| KAFAS  | Speed Limit Information:<br>SLI_ON_OFF: RR01_off (00) -> F056 (01)<br>No pass info:<br>NPI_ON_OFF: RR01_off (00) -> F056 (01)<br>Pedestrian Alert:<br>PPP_SW_MODULE_ON_OFF: all_others (00)	-> F030 (01)<br>Admin:<br>CODING_DATE will update  | Lane Departure Warning:<br>SEND_MSG_18A: message_off (00) -> message_on (01)<br>SEND_MSG_327: message_off (00) -> message_on (01)<br>TLC_ON_OFF: TLC_off (00) -> TLC_on_F020_F030_F025 (01)<br> ACTUATOR_ON_OFF: all_others (00) -> vib_warn_on_F20_F30_F25 (01)<br>VIBRATION_VALUE: all_others (01,01,01,01,01,01,01,01) -> F045 (04,05,06,07,08,09,0A,0B)<br>Car Speed Warning (adds vibration):<br>CAR_SPEED_VIBRATION: F020 (3C,50,64,78,8C,A0,B4,DC) -> F001 (3C,50,64,78,8C,A0,B4,C8)|
| BDC  | High Beam Assist (effect to be seen):<br> C_HBA_CAM_POS_Z: F015_KafasLow (75)	-> F015_KafasHigh (7E)<br>Pedestrian Alert:<br>PERSONEN_WARNUNG_TAG: nicht_aktiv (00) ->	aktiv (01)| Lane Depature Warning:<br>ComAdapterPdu_18A_Steuerung_Vibration_Lenkrad: nichtaktiv (00) -> aktiv (01)<br> SENDER_QU_VIB_AMP: ST_VIB_AMP_LRE (00) -> NCD2: ST_VIB_AMP_EPS (01)<br>SPURVERLASSENS_WARNUNG: nichtaktiv (00) -> aktiv (01)<br>Heated Steering Wheel message:<br>LHZ_CCM: nicht_aktiv (00) -> aktiv (01)<br>Unknown:<br>AMBIENTE_FES_LAYER: Layer_1 (01) -> inaktiv (00)<br>PIA_DEFAULT_CS: Farbe_1 (01)	-> Farbwelt3_Farbe1 (31) |

### Final FDL edit
#### Lane Departure Warning
- KAFAS: Use 0321 VO coded version is fine
- HU:
  - Use 0321 VO coded version
  - FDL: SPURVERLASSENSWARNZEITPUNKT = gen_1 (01)
- BDC:
  - Code back to 2020 5AS VO version
  - FDL:
    1. ComAdapterPdu_18A_Steuerung_Vibration_Lenkrad = aktiv (01)
    2. ComAdapterPdu_327_Steuerung_Vibration_Lenkrad = aktiv (01) (maybe already aktiv)
    3. PIA_DEFAULT_SPURVERLASSENS_WARNUNG = aktiv (01)
    4. SPURVERLASSENS_WARNUNG = aktiv (01)
- KOMBI:
  - Code back to 2020 5AS VO version
  - FDL:
    1. TLC_VERBAUT = aktiv (01)
    1. ST_TLC_TIMEOUT: nicht_aktiv (00) -> aktiv (01)
    1. ST_TLC_ALIVE: nicht_aktiv (00) -> aktiv (01)
    1. ST_TLC_APPL: nicht_aktiv (00) -> aktiv (01)  
    Maybe also need:
    1. ST_DISP_BLTW_TIMEOUT: nicht_aktiv (00) -> aktiv (01)
    1. ST_DISP_BLTW_APPL: nicht_aktiv (00) -> aktiv (01)
    1. ST_CS_TIMEOUT: nicht_aktiv (00) -> aktiv (01)
    1. ST_CS_APPL: nicht_aktiv (00) -> aktiv (01)
#### New UI Look
- HU: HMI_ID_VERSION: id5_plus_plus (02) -> id6_light
- KOMBI: 
  NAVICOLOR_TABLE: farbwelt_id5 (02) -> farbtabelle_2103
  FARBSET_NEU: alt (00) -> neu (01)  
N.B: After upgrade to ID6_LITE, following event view of the central ring will disappear in setting menu (what a shame):
- Auto Start/Stop (I quite like it...)
- Driving mode (Also quite like it...)
- Menu selection (doesn't care)
#### Sports Display
HU/3000/HUD_SPORTANZEIGE_ENABLE: Aktiv  
HU/3000/HUD_SPORTANZEIGE_FES_ENABLE: Aktiv  
HU/3000/HUD_SPORTANZEIGE_MS_GASSE_ENABLE: Aktiv  
KOMBI/3003/HUD_SPORTANZEIGE_ENABLE: Aktiv  
KOMBI/3003/HUD_SPORTANZEIGE_FES_ENABLE: Aktiv  
KOMBI/3003/HUD_SPORTANZEIGE_MS_GASSE_ENABLE: Aktiv  
KOMBI/3003/HUD_FAHRSTUFE_ENABLE: Aktiv  
#### Remember last driving mode
BDC/PfFesMaster/FesAllowMode2LastFunction: Aktiv  
BDC/PfFesMaster/FesAllowMode4LastFunction: Aktiv  
BDC/PfFesMaster/FesAllowMode7LastFunction: Aktiv  
BDC/PfFesMaster/FesLastFunctionInterval: Immer_aktiv (FF)  
#### Auto-fold mirror (implemented by change delay to 0sec, previously need to hold lock on keyfob by 1.5sec)
BDC/3056/KOMFORT_SCHLIESSEN: 0x00
#### Functions seems not possible to code:
- Lane Departure Warning in HUD (doesn't seem possible in MINI F60)
- Remember High Beam Assist last setting

