# TrueNAS Performance and Power Test on BeeLink ME Mini

The test should give some guidelines and hints only and is not intened to be complete or high tuned.

## Test Setup

* Beelink ME Mini with 12GB RAM with [BIOS M1V305](https://dr.bee-link.cn/?dir=uploads%2FMEmini)
* 6x Samsung 990 Pro SSD 2TB
* Debian Trixie (13) on eMMC for having a second OS (incl WLAN)
* TrueNas Community Edition 25.4.2.1
  * Installed on nvme03 (nvme slot 4) 
  * Partitioned: OS 32GB, apps-pool 700GB, data-pool 1.05TB
* big-pool: 5x SSD, draid1, 7.13TB

To measure the power, two mechanisms were used, the table shows the hardware version.

* A bash script that uses `energy_uJ` data gathered by the OS where two measures need to be taken and some math applied, details on request. For an idle BeeLink ME Mini, the data is quite accurate but not when under load. In idle mode, the ME Mini uses about ~10W (!) with the setup described above.
* A harware plug where the ME Mini is connected to. This data is accurate not only in idle but also in load mode.

Note that when under load, the fan was doing only a bit more work but still being very silent. I purchased special heatpads for the SSD's with 13W/mk (usual it is 6W/mk) fearing that the system may overheat by the Samsung PRO SSD's. I will install them when they arrive but it seems that this purchase was not necessary.

## Transfer/Power Test Without Network

This test measures the internal transfer rate only without any network IO.

First change to the pool you want to test such as `cd /mnt/big-pool`
Then run the test, change `bs` and `readwrite` accordingly:

```
sudo fio --ramp_time=5 --gtod_reduce=1 --numjobs=1 --bs=1M --size=100G --runtime=60s --readwrite=read --name=testfile
```

**Result**

| **Pool**  | **Read or Write** | **Blocksize** | **Power max [W]** | **MB/s** | **IOPS** |
|----------:|:-----------------:|--------------:|:-----------------:|---------:|---------:|
| big-pool  | w                 | 1M            | 36                | 432      | 411      |
| big-pool  | w                 | 100K          | 36                | 685      | 6.687    |
| big-pool  | r                 | 1M            | 33                | 1.670    | 1.592    |
| big-pool  | r                 | 100K          | 33                | 1.887    | 18.40    |
| data-pool | w                 | 1M            | 26                | 450      | 472      |
| data-pool | w                 | 100K          | 26                | 951      | 9.283    |
| data-pool | r                 | 1M            | 22                | 1.282    | 1.222    |
| data-pool | r                 | 100K          | 22                | 2.067    | 20.200   |

