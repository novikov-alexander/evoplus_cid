# evoplus_cid

Tool to change the CID on Samsung Evo Plus SD Cards. Requires rooted Android device.
Precompiled Android binary included. May also work on regular Linux, but you must use
a real SD controller, not a USB mass storage type SD reader.
See http://richard.burtons.org/2016/07/01/changing-the-cid-on-an-sd-card/
for more details.

**USE AT YOU OWN RISK!**

## Usage:
```
./evoplus_cid <device> <cid> [serial]
device - SD card block device, e.g. /dev/block/mmcblk0
cid - new CID, must be in hex (without 0x prefix)
  it can be 32 chars with checksum or 30 chars without, it will
  be updated with new serial number if supplied, the checksum is
  (re)calculated if not supplied or new serial applied
serial - optional, can be hex (0x prefixed) or decimal
  and will be applied to the supplied cid before writing
```

## Backup Original CID

Before changing the CID, it is highly recommended to back up the original CID. You can read the current CID from `/sys/block/mmcblk0/device/cid`:

1. Ensure the SD card is inserted and detected by the system.
2. Run the following command to read the CID:

   ```bash
   cat /sys/block/mmcblk0/device/cid
   ```

   Replace `mmcblk0` with the appropriate device name if necessary (e.g., `mmcblk1`).

3. Save the output (a 32-character hexadecimal string) in a safe place. This is your original CID.

Example:
```bash
$ cat /sys/block/mmcblk0/device/cid
1b534d3030303030100e5dd998014300
```

## Compile

`gcc evoplus_cid.c -o evoplus_cid`

You can safely ignore compilation warnings.

---

## Notes

- **Root Access Required:** This tool requires root privileges to access the SD card block device.
- **Use Real SD Controller:** The tool may not work with USB-based SD card readers. Use a built-in SD controller (e.g., on old laptops, Android devices or Raspberry Pi).
- **Backup First:** Always back up your original CID before making any changes.

---

Based on the reverse engineering and code of Sean Beaupre,
see: https://github.com/beaups/SamsungCID
