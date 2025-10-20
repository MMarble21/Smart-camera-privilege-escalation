# Vulnerability Disclosure: Privilege Escalation in Nous W3 Smart WiFi Camera

**CVE ID:** CVE-2025-56438 (Assigned by MITRE)

---

## Summary

An issue in the firmware update mechanism of Nous W3 Smart WiFi Camera
v1.33.50.82 allows unauthenticated and physically proximate attackers 
to escalate privileges to root via supplying a crafted update.tar
archive file stored on a FAT32-formatted SD card.


## Vulnerability Details

*   **Vulnerability Type:** Insufficient Verification of Data Authenticity
*   **Vendor:** Nous technology
*   **Affected Product:** Nous W3 Smart WiFi Camera
*   **Affected Firmware Version:** 1.33.50.82
*   **Affected Bundle:** TUYA_AK3918EV330

### Affected Components
The following scripts and mechanisms are involved in the vulnerability:
anyka_service.sh, tf_update.sh, update.sh, update_config.sh, /mnt update mechanism, /etc/shadow

### Detailed Description
The exploit does not require network access or authentication; physical access to the SD card slot is sufficient.
The existing verification mechanism (bundle, version, md5) does not appropriately validate the authenticity of the update package.
The tested payload modifies /etc/shadow to enable root access with a known password, but arbitrary shell commands can be executed.
Potentially exploitable on other Anyka-based devices using a similar SD card firmware update mechanism.
### Attack Vector
Physical insertion of an SD (TF) card containing a malicious `update.tar` archive with a crafted `update_config.sh` script. The script is executed automatically with `root` privileges during device boot or update, without user interaction or the need for network access or authentication.

### Impact
*   Code execution (as `root`)
*   Privilege escalation (to `root`)
*   Information disclosure (with potential privacy impact due to unauthorized access to camera video stream)
*   Permanent modification of system configuration.
*   Full device control.
  
### Proof of Concept (Image)

Due to the lack of cooperation from the vendor, an image is provided as proof of the exploit's effectiveness.
The following image shows the results of the attack, demonstrating successful root access.
![Screenshot PoC Privilege Escalation](assets/PoC(2).png)
(Image link: [https://github.com/MMarble21/Smart-camera-privilege-escalation/blob/main/assets/PoC(2).png](https://github.com/MMarble21/Smart-camera-privilege-escalation/blob/main/assets/PoC(2).png))

### Mitigation/Solution
Currently, no official patch has been released by the vendor. Users are advised to limit physical access to devices, if possible.

### Disclosure Timeline

-18 July 2025: Disclosure of the Insufficient Verification of Data Authenticity vulnerability to Nous Technology. 

-18 July 2025: Obtained CVE Request ID from MITRE.

-22 July 2025: Vendor response: Port intended for initial configuration use only. 

-23 July 2025: Follow up: Reiterated that the vulnerability occurs on a device already in use, not just during initial setup. 

-30 September 2025: Obtained CVE-2025-56438 from MITRE. 

-20 October 2025: Publication of the vulnerability.

This vulnerability is being disclosed in accordance with a 90-day coordinated disclosure policy. The information contained in this report was not made public until the deadline was met or a fix was released. As there has been no cooperation from the vendor, the details are now being made public.

### Credits

[MMarble21]
