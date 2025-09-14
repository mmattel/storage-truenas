# Installing and Managing TrueNAS on Beelink ME Mini

## General

TrueNAS is the perfect storage software when installed on a Beelink ME Mini.

Before starting any installation, read the [Beelink ME Mini preparation page](../beelink_me_mini/beelink-me-mini.md). This will help you avoid potential issues.

## Assumptions

Read the following assumtions and adapt to your environment:

* You have prepared your Beelink ME Mini, **especially** the NVMe and BIOS section as linked above.
* TrueNAS will be installed at NVME slot4.\
This is the recommended location according Beelink when not using eMMC.\
NVME slot4 uses PCIe 3.0 double speed and the NVMEs have greatly improved duration and performance compared to eMMC. 
  * The boot device (/dev/nvme3n1) will be partitioned the following way:
    *  32GB for TrueNAS
    * 712GB ZFS pool for apps
    * 1.1TB ZFS pool for apps data
* You create one big ZFS data pool consisting of the remaining NVMEs which will give you 7.8TB usable space for data sets.
  * Note that I have used draid 
