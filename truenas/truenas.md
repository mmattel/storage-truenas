# Installing and Managing TrueNAS on Beelink ME Mini

   * [General](#general)
   * [Assumptions](#assumptions)
   * [TrueNAS Installation](#truenas-installation)

## General

TrueNAS is the perfect storage software when installed on a Beelink ME Mini.

Before starting any installation, read the [Beelink ME Mini preparation page](../beelink_me_mini/beelink-me-mini.md). This will help you avoid potential issues.

## Assumptions

Read the following assumtions and adapt to your environment:

**Overview:**

* eMMC &emsp; &emsp; &emsp; &emsp; &nbsp; --> &emsp; &emsp; Debian
* NVMEs
  * boot-pool: &emsp; &nbsp; 32GB &emsp; (no raid protection)
  * apps-pool: &emsp; 712TB &emsp; (no raid protection)
  * data-pool: &emsp; &nbsp; 1.1TB &emsp; (no raid protection)
  * big-pool: &emsp; &ensp; &nbsp; 7.8TB &emsp; (draid1 protected)

**Details:**

* You have prepared your Beelink ME Mini, **especially** the NVMe (2TB sticks) and BIOS section as linked above.
* TrueNAS will be installed at NVME slot4.\
This is the recommended location according Beelink when not using eMMC.\
NVME slot4 uses PCIe 3.0 double speed and the NVMEs have greatly improved duration and performance compared to eMMC. 
  * The boot device (`sudo nvme list` --> `/dev/nvme3n1`) will be partitioned the following way:
    *  32GB for TrueNAS (1)
    * 712GB ZFS pool for apps (2)
    * 1.1TB ZFS pool for apps data (2)
* You create one big ZFS data pool consisting of the remaining 5 NVMEs which will give you 7.8TB usable space for data sets.
  * Note that I have configured draid1 = one distributed parity over 5 disks.
* Installed Debian on eMMC for emergency purposes, which is not covered here.

1 ... Needs manual installation intervention for partitioning.\
2 ... Note that the ZPOOL will be created manually on the console and imported by TrueNAS.

## TrueNAS Installation

asd
