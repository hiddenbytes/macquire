# Introduction
Macquire (**MAC**intoshOS A**quire**) is an experimental tool created at HiddenBytes to perform a logical acquisition from a live MacOS system. 
This is provided on an "AS-IS" basis, and is not a substitute for any commercial offerings.
The resulting file is a non-proprietary DMG file that can be subsequently analysed using the examiner's preferred tool or method.
It remains the sole responsibility of the user to verify and validate this tool to ensure that the impact and limitations of this tool is understood prior to use.

# **Prerequisites**
- The password for an administrator account on the suspect (target) device
- Suitably sized destination media to store acquired image.
   This should be a sterile SSD that is equal to, or larger than the capacity of the physical disk (for example, a 1TB Macbook Pro would require a destination disk of at least 1TB) 

# **Instructions**
The preperation is split into before the scene (ie: performed on an examiner's device), and on scene (performed on the subject's device). 

## Phase 1: Prepare the destination media
_This should be performed from a known good device prior to attending the scene. **DO NOT PERFORM PHASE 1 ON THE SUSPECT'S DEVICE**_

1) Prior to storing evidence onto the destination volume, the examiner should ensure no unrelated data is present. This can be achieved by sterilising the destination media.
   - Wipe all sectors with 0x00s
   - Verify the sterilisation of the media by performing a Checksum64 calculation on the media.
     The expected checksum should be all 0s

     <img width="440" alt="image" src="https://github.com/hiddenbytes/macquire/assets/60643888/db721414-19a6-497a-b2e2-127513ea9948">

     
2) Once verified, partition (format) the destination media with a MacOS native filesystem.
   This can be performed using the Apple Disk Utility.
   Disk Utility -> Right Click the previously sterilised drive (from Step 1) -> "Erase" 
         HFS+ (Mac OS Extended (Journaled)) is recommended as it is considered more robust than APFS and ExFAT, and ensures the Apple Extended Metadata information is retained.
         It is recommended that the name of the destination volume is kept short, without any special characters (including spaces)


    <img width="487" alt="image" src="https://github.com/hiddenbytes/macquire/assets/60643888/fabd6cf3-e77d-407a-8d10-4ae88d839374">


3) Copy the MacquireCLI script into the root folder of the newly partitioned volume.

## Phase 2: Running Macquire (At the scene)
_The following steps are performed from the subject's device_
Modifications will be made to the filesystem. Contemperaneous notes and adequate documentation should be maintained.

1) Grant "Terminal" with Full Disk Access.
    Click on the Apple icon in the top left of your menu bar -> Click "System Setings" -> Click "Privacy & Security".
    Locate "Full Disk Access", and click the "+".
    Enter the administrator password when prompted.
    Click "Applications" -> "Utilities" -> "Terminal".
    Close the settings dialogue.

   <img width="719" alt="image" src="https://github.com/hiddenbytes/macquire/assets/60643888/e77235dc-cb3d-4735-9cbb-5bddbd31ac23">


3) Plug in the sterilised destination media created in Phase 1.
    On newer MacOS operating systems, you may be prompted to "Allow Accessory to Connect".
    Select Allow if prompted.

4) Open a new terminal, and verify that terminal session is running as an administrator on the device.
   If the terminal is currently running from a user without administrator privileges, switch to the superuser by typing the following

       `sudo su`

   If prompted for an administrator password, enter the password of an administrator on the device.
  
5) Navigate to the Volume of the sterilised drive
   
       `cd /Volumes/<Destination Volume created earlier>`


6) Run MacquireCLI
_Note: The name of the tool will be subject to change. If you are not sure of the name of the tool, run 'ls' to list the files on the Volume_

       `python3 macquireCLI_public_v1.0.py`

<img width="606" alt="image" src="https://github.com/hiddenbytes/macquire/assets/60643888/90f28831-869f-4798-adf8-906b7c8f4885">


7) Enter the information as prompted
    Case Reference - This should be a unique name for allowing the investigator to identify the casefile
    Examiner Name - This is the name of the examiner, or person performing the acquisition
    Case Reference Folder - By default, the tool will create a new folder using the case reference provided.
                               If for any reason an examiner would like to save the output to another location, they can enter the path manually.

9) Allow the tool to run. This process may take a while.
   
10) When the tool has finished, a MD5 and SHA1 hash of the output will be shown on terminal.
    In addition, several new files will be created on the destination drive within a new folder called (Case Reference).
    Within this folder will be the following:
        Snapshot folder - This is no longer required, and should be empty.
        (Case Reference).dmg - This is the read-only DMG file containing the logical acquisition
        (Case Reference)_Summary.txt - This is a summary of the acquisition, including timestamps
        Hash.txt - This is a hash output of the DMG file and the summary file

11) Once the tool has finished (the terminal shows the MD5 and SHA1 hash of the output), you can close the terminal and eject the device
    
12) The resulting DMG can now be imported into the examiner's tool of choice.

# Feedback / Questions

For any feedback/ enquiries, please contact Hello[at]hiddenbytes.co.uk.
