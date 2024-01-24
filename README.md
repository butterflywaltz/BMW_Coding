# BMW_Coding
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
- Need ISTA+ for programming (don't know how to use e-sys to program), this will take about 380G of disk spaces, so definitely go for 1T SSD drives
- Need e-sys for VO and FDL coding
- Need FSC codes (7E and BF)
- Need a Ethernet to OBD2 cable
