# Echo-Show-8-IoT-Evidence-Investigation
Forensic examination of an Amazon Echo Show 8 eMMC training image on Kali Linux. Covers device identity, Fire OS configuration, user data, Wi-Fi and Bluetooth, web history, a carved call log, and multimedia evidence, with a findings classification and chronology. 
Training exercise only. No cloud acquisition, account access or live-device contact was involved. The findings describe what the device recorded, not who used it.
Case Question
What can the Echo Show 8 eMMC and userdata evidence establish about device identity, the operating system, user data, network and web activity, phone communication, multimedia and the sequence of events?
Evidence and Environment
•	Image: EMMC_8GTF4R_ROM1_000000000000_0001D2000000.bin (hash verified against the instruction file; the image is not included in this repository)
•	Partitions: p21 is the system partition and p23 is the userdata partition
•	Platform: Kali Linux on VMware Workstation
•	Tools: losetup, mmls, dd, bulk_extractor, scalpel, foremost, PhotoRec, exiftool
Method
1.	Verify the image hash, then attach the image as a read-only loop device.
2.	Map the partitions and mount the userdata partition with noload so the ext4 journal isn't replayed.
3.	Analyse the system and user artefacts and run bulk_extractor.
4.	Isolate the userdata partition with dd, then carve deleted files from it.
sudo losetup --partscan --find --show --read-only <image>.bin
sudo mount -o ro,noload /dev/loopXp23 /mnt/echoshow8-2
dd if=<image>.bin of=userdata_partition.bin bs=512 skip=7929856 count=7339999
foremost -t jpg,mp4 -i userdata_partition.bin -o foremost_output
Key Findings
Area	Finding
OS and device	Fire OS (Android-based); device name, serial number and Bluetooth MAC address recovered
User and account	One primary user (ID 0); email address and physical address linked to the Amazon account
Network	Saved Wi-Fi network; paired and connected Bluetooth devices
Web	Browser visits, keyword and YouTube searches, a Microsoft Live sign-in
Phone	A deleted call record to the contact 'hibna', recovered by carving XML files
Multimedia	12 JPEGs on the file system; a camera image recovered only by carving; 6 photos and 2 videos uploaded; video and music playback recorded

Limitations
•	Carved files may lose their original names and timestamps.
•	The uploaded videos weren't recovered from the device and would need cloud data.
•	The evidence links the device to an account, not to a specific person.
Lessons Learned
•	Carving was essential, because the deleted phone logs and camera image were recovered only from unallocated space.
•	foremost outperformed scalpel, which produced more than 27 GB of duplicate output from a 7.3 GB image.
Repository Contents
•	report/: the full forensic report (PDF, redacted)
•	screenshots/: tool output (redacted)
Author Favour Ifeyinwa Madu  
Instructor: Aminu Idris, AMCPN (ICDFA) · October 2026


