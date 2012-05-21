* Nagios Plug-in for predicting Disk Full

This plug-in (and accompanying cron job) tries hard to predict a "disk
full" scenario well before the disk actually fills up.  Typically your
disks are running at low utilization, maybe 20% full, maybe with 200Gb
free space.  Suddenly the disks start to fill up for some reason. Maybe
someone forgot to turn off debug logging, or some script kiddie is
uploading warez, or a sysadmin is running an out-of-the-ordinary job.

Anyway, your disks are well below the nagios threshold of 90%, so for
nagios, all is green.

The disk-fill-rate plug-in will check to see the _rate_ at which the
disks are being filled, and if it is consistently being filled at a rate
which indicates that (if the rate is sustained) a disk full will happen
within a certain number of hours, it will raise a warning or critical,
depending on how short time it is until the disk would become full.

** How it works

A cron job that runs every minute keeps a log of how much free space
is available, and keeps that in a small text file, one line per mounted
volume.

The check\_disk\_fill\_rate plug-in then reads the last few entries
(five minutes) of this log and (for each volume) calculates how fast
the free space is dwindling, and uses the score with the "slowest" rate
(i.e. the most conservative) and compares that with the thresholds.

The response is critical if any disk is filling up within a critical
threshold, otherwise, a warning threshold if any disk is filling up
within a warning threshold, otherwise normal.

