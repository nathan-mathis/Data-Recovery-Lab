# Forensic Data Recovery - 14-Year-Old College Hard Drive

**A real-world, forensic-style data recovery of a 160GB SATA hard drive that had been sitting exposed in storage for over a decade - recovered with zero read errors using a methodical, preservation-first approach.**

Built by [Nathan Mathis](https://www.linkedin.com/in/n-mathis/) | IT Professional | 14+ Years Enterprise IT Experience

---

## The Story

I found an old SATA hard drive in a box in my closet. No case. No protection. Just sitting there next to a pile of wool socks. I hadn't seen it in over a decade.

Then it clicked - this was my college drive. Photos, documents, music - everything from that period of my life was likely still on it.

But I couldn't just plug it into my current system and hope for the best. Modern operating systems are designed to "help" - repairing, initializing, or writing to disks automatically. In this situation, that could have destroyed the very data I was trying to recover.

So I stopped. Instead of rushing it, I decided to treat this like a forensic recovery project - not just pulling files off a drive, but doing it without risking the data.

---

## The Approach

I removed my main operating system from the equation entirely. Instead, I booted into a Linux live environment to create a controlled, isolated space where I could interact with the drive without risking unintended writes.

The principles driving every decision:

- **Isolation** - Boot from a live, read-only OS so the source drive is never touched by the primary system
- **Assessment** - Check the drive's health via SMART data before attempting anything
- **Imaging** - Create a complete sector-by-sector image to preserve the original data structure
- **Verification** - Mount the image read-only and spot-check recovered files for integrity
- **Preservation** - Maintain the original image and logs as a forensic-grade backup

---

## Hardware Setup

| Component | Details |
|-----------|---------|
| **Source Drive** | 160GB SATA HDD (14+ years old, no enclosure) |
| **Boot Environment** | Ubuntu Live USB |
| **SATA Interface** | Plugable SATA dock (external connection) |
| **Destination** | External SSD for imaging and recovery |
| **Host Machine** | AMD Ryzen 7 7800X3D, 32GB RAM, NVMe Storage |

---

## The Process

### Step 1: Boot into Ubuntu Live USB
Booted into a clean, isolated Linux environment. The source drive was never exposed to my primary Windows OS.

### Step 2: Identify Drives
```bash
lsblk -f
```
Confirmed source drive (old HDD) vs. destination drive (recovery SSD) by device name and size. No room for mistakes here.

### Step 3: Assess Drive Health
```bash
sudo smartctl -a /dev/sdX
```
Examined SMART data for reallocated sectors, pending sectors, and any signs of mechanical degradation. This determines whether aggressive recovery passes are safe to attempt.

### Step 4: Full Disk Imaging with ddrescue
```bash
sudo ddrescue -n /dev/sdX /media/ubuntu/RecoverySSD/ddrescue/disk.img /media/ubuntu/RecoverySSD/ddrescue/disk.log
```
Created a complete, sector-by-sector image of the entire drive. Monitored the live output for:
- Increasing "rescued" value
- Stable "current rate"
- Low or unchanging error count

The `-n` flag skips bad sector retries on the first pass, prioritizing the bulk of recoverable data before going back for problem areas.

### Step 5: Disconnect Source Drive
Physically removed the original HDD from the system. From this point forward, all work happens against the image - never the original.

### Step 6: Mount Image Read-Only
```bash
sudo mkdir /mnt/image
sudo mount -o ro,loop /media/ubuntu/RecoverySSD/ddrescue/disk.img /mnt/image
```
Read-only mount ensures the recovered image can't be accidentally modified during extraction.

### Step 7: Extract Recovered Files
```bash
sudo mkdir -p /media/ubuntu/RecoverySSD/recovered
rsync -avh --progress /mnt/image/ /media/ubuntu/RecoverySSD/recovered/
```
Copied all recovered data to the destination SSD with progress monitoring and integrity verification.

### Step 8: Clean Shutdown
Unmounted the image, shut down the Ubuntu Live session cleanly, and removed the Live USB.

### Step 9: Access from Primary System
Booted into Windows and copied the recovered files from the external SSD to permanent storage.

---

## The Result

**Full recovery. Zero read errors. Every file intact.**

After sitting exposed in a box for over a decade, the entire drive was successfully imaged and all data recovered. Photos, documents, music - a complete digital time capsule from college, fully accessible again.

Because the recovery captured a full disk image, the entire environment is preserved exactly as it existed. The image can be loaded into a virtual environment and explored as if that system never went away.

---

## What This Project Became

What started as curiosity about an old hard drive became a turning point. The forensic mindset - preserving the original state, working from an image, never altering the source - sparked a deeper interest in Digital Forensics and Incident Response (DFIR).

The same principles that made this recovery successful are the foundation of professional forensic investigation: chain of custody, evidence preservation, and methodical documentation.

---

## Key Skills Demonstrated

- Forensic-style evidence preservation workflow
- Linux live environment for isolated, controlled access
- Drive health assessment via SMART data (smartctl)
- Sector-by-sector disk imaging (ddrescue)
- Read-only image mounting and verification
- File extraction and integrity checking (rsync)
- Documentation of recovery procedures and outcomes
- Hardware handling and SATA interface management

---

## Tools Used

| Tool | Purpose |
|------|---------|
| **Ubuntu Live USB** | Isolated boot environment - no writes to source |
| **lsblk** | Drive identification and filesystem detection |
| **smartctl** | SMART health assessment of source drive |
| **ddrescue** | Sector-by-sector forensic disk imaging |
| **mount (ro,loop)** | Read-only image mounting for safe extraction |
| **rsync** | File extraction with progress and verification |
| **Plugable SATA dock** | External drive interface for safe connection |

---

## What's Next

- [ ] Cross-reference with DFIR lab for evidence preservation workflows
- [ ] Practice recovery on intentionally corrupted file systems
- [ ] Explore RAID recovery scenarios
- [ ] Build a technician-facing recovery decision tree
- [ ] Document SSD-specific recovery challenges (TRIM complications)

---

## About Me

IT professional with 14+ years of enterprise experience in IMACD (Install, Move, Add, Change, Dispose) service delivery at Dell Technologies supporting Boeing facilities. CompTIA A+ certified, currently pursuing Network+ (N10-009), with a long-term career path toward Digital Forensics and Incident Response (DFIR).

- [LinkedIn](https://www.linkedin.com/in/n-mathis/)
- [Resume available upon request]
