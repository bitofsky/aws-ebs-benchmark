# AWS EBS GP2 vs GP3

https://docs.aws.amazon.com/ko_kr/AWSEC2/latest/UserGuide/benchmark_procedures.html

## Test Spec
* AWS EKS 1.19 / r5.large
* fio 3.16
* GP2 : 1TB
* GP3 : 1TB (3000IOPS, 250MB)

# GP2 Random Read
```bash
root@test-0:/# fio --directory=/pvc/gp2 --name rread --direct=1 --rw=randread --bs=16k --size=1G --numjobs=16 --time_based --runtime=180 --group_reporting --norandommap
```
```
rread: (g=0): rw=randread, bs=(R) 16.0KiB-16.0KiB, (W) 16.0KiB-16.0KiB, (T) 16.0KiB-16.0KiB, ioengine=psync, iodepth=1
Jobs: 16 (f=16): [r(16)][100.0%][r=47.0MiB/s][r=3008 IOPS][eta 00m:00s]
rread: (groupid=0, jobs=16): err= 0: pid=694: Fri May  7 04:25:07 2021
  read: IOPS=3023, BW=47.2MiB/s (49.5MB/s)(8503MiB/180005msec)
    clat (usec): min=240, max=34053, avg=5291.19, stdev=737.18
     lat (usec): min=240, max=34053, avg=5291.29, stdev=737.17
    clat percentiles (usec):
     |  1.00th=[ 2802],  5.00th=[ 4817], 10.00th=[ 4948], 20.00th=[ 5014],
     | 30.00th=[ 5145], 40.00th=[ 5211], 50.00th=[ 5276], 60.00th=[ 5342],
     | 70.00th=[ 5407], 80.00th=[ 5538], 90.00th=[ 5735], 95.00th=[ 5932],
     | 99.00th=[ 6849], 99.50th=[ 7570], 99.90th=[11863], 99.95th=[14353],
     | 99.99th=[22676]
   bw (  KiB/s): min=45434, max=146122, per=99.99%, avg=48365.49, stdev=331.31, samples=5759
   iops        : min= 2839, max= 9132, avg=3022.34, stdev=20.71, samples=5759
  lat (usec)   : 250=0.01%, 500=0.40%, 750=0.30%, 1000=0.07%
  lat (msec)   : 2=0.13%, 4=0.45%, 10=98.48%, 20=0.16%, 50=0.02%
  cpu          : usr=0.06%, sys=0.15%, ctx=546245, majf=0, minf=218
  IO depths    : 1=100.0%, 2=0.0%, 4=0.0%, 8=0.0%, 16=0.0%, 32=0.0%, >=64=0.0%
     submit    : 0=0.0%, 4=100.0%, 8=0.0%, 16=0.0%, 32=0.0%, 64=0.0%, >=64=0.0%
     complete  : 0=0.0%, 4=100.0%, 8=0.0%, 16=0.0%, 32=0.0%, 64=0.0%, >=64=0.0%
     issued rwts: total=544207,0,0,0 short=0,0,0,0 dropped=0,0,0,0
     latency   : target=0, window=0, percentile=100.00%, depth=1
 
Run status group 0 (all jobs):
   READ: bw=47.2MiB/s (49.5MB/s), 47.2MiB/s-47.2MiB/s (49.5MB/s-49.5MB/s), io=8503MiB (8916MB), run=180005-180005msec
 
Disk stats (read/write):
  nvme7n1: ios=543947/11850, merge=0/111, ticks=2856894/80602, in_queue=2198228, util=100.00%
```

# GP3 Random Read
```bash
root@test-0:/# fio --directory=/pvc/gp3 --name rread --direct=1 --rw=randread --bs=16k --size=1G --numjobs=16 --time_based --runtime=180 --group_reporting --norandommap
```
```
rread: (g=0): rw=randread, bs=(R) 16.0KiB-16.0KiB, (W) 16.0KiB-16.0KiB, (T) 16.0KiB-16.0KiB, ioengine=psync, iodepth=1
Jobs: 16 (f=16): [r(16)][100.0%][r=46.9MiB/s][r=2999 IOPS][eta 00m:00s]
rread: (groupid=0, jobs=16): err= 0: pid=750: Fri May  7 04:33:19 2021
  read: IOPS=3016, BW=47.1MiB/s (49.4MB/s)(8484MiB/180005msec)
    clat (usec): min=379, max=40194, avg=5302.82, stdev=619.41
     lat (usec): min=379, max=40194, avg=5302.93, stdev=619.40
    clat percentiles (usec):
     |  1.00th=[ 2933],  5.00th=[ 5014], 10.00th=[ 5145], 20.00th=[ 5211],
     | 30.00th=[ 5276], 40.00th=[ 5276], 50.00th=[ 5342], 60.00th=[ 5342],
     | 70.00th=[ 5407], 80.00th=[ 5407], 90.00th=[ 5473], 95.00th=[ 5604],
     | 99.00th=[ 5997], 99.50th=[ 7046], 99.90th=[11076], 99.95th=[13042],
     | 99.99th=[20317]
   bw (  KiB/s): min=45614, max=143776, per=99.99%, avg=48256.84, stdev=318.64, samples=5755
   iops        : min= 2849, max= 8986, avg=3015.47, stdev=19.92, samples=5755
  lat (usec)   : 500=0.04%, 750=0.51%, 1000=0.16%
  lat (msec)   : 2=0.12%, 4=0.47%, 10=98.54%, 20=0.14%, 50=0.01%
  cpu          : usr=0.06%, sys=0.16%, ctx=545001, majf=0, minf=245
  IO depths    : 1=100.0%, 2=0.0%, 4=0.0%, 8=0.0%, 16=0.0%, 32=0.0%, >=64=0.0%
     submit    : 0=0.0%, 4=100.0%, 8=0.0%, 16=0.0%, 32=0.0%, 64=0.0%, >=64=0.0%
     complete  : 0=0.0%, 4=100.0%, 8=0.0%, 16=0.0%, 32=0.0%, 64=0.0%, >=64=0.0%
     issued rwts: total=542994,0,0,0 short=0,0,0,0 dropped=0,0,0,0
     latency   : target=0, window=0, percentile=100.00%, depth=1
 
Run status group 0 (all jobs):
   READ: bw=47.1MiB/s (49.4MB/s), 47.1MiB/s-47.1MiB/s (49.4MB/s-49.4MB/s), io=8484MiB (8896MB), run=180005-180005msec
 
Disk stats (read/write):
  nvme8n1: ios=542510/11, merge=0/30, ticks=2856038/79, in_queue=2139844, util=100.00%
```

# GP2 Random Write
```bash
root@test-0:/# fio --directory=/pvc/gp2 --ioengine=psync --name rwrite --direct=1 --rw=randwrite --bs=16k --size=1G --numjobs=16 --time_based --runtime=180 --group_reporting --norandommap
```
```
rwrite: (g=0): rw=randwrite, bs=(R) 16.0KiB-16.0KiB, (W) 16.0KiB-16.0KiB, (T) 16.0KiB-16.0KiB, ioengine=psync, iodepth=1
Jobs: 16 (f=16): [w(16)][100.0%][w=46.0MiB/s][w=2944 IOPS][eta 00m:00s]
rwrite: (groupid=0, jobs=16): err= 0: pid=720: Fri May  7 04:28:07 2021
  write: IOPS=2980, BW=46.6MiB/s (48.8MB/s)(8384MiB/180005msec); 0 zone resets
    clat (usec): min=522, max=64089, avg=5365.60, stdev=1082.55
     lat (usec): min=523, max=64089, avg=5366.11, stdev=1082.65
    clat percentiles (usec):
     |  1.00th=[ 2573],  5.00th=[ 4686], 10.00th=[ 4817], 20.00th=[ 5014],
     | 30.00th=[ 5080], 40.00th=[ 5145], 50.00th=[ 5211], 60.00th=[ 5276],
     | 70.00th=[ 5342], 80.00th=[ 5473], 90.00th=[ 5735], 95.00th=[ 7767],
     | 99.00th=[ 8979], 99.50th=[10028], 99.90th=[14222], 99.95th=[17171],
     | 99.99th=[27657]
   bw (  KiB/s): min=36000, max=106290, per=99.99%, avg=47688.90, stdev=209.48, samples=5759
   iops        : min= 2250, max= 6643, avg=2980.03, stdev=13.10, samples=5759
  lat (usec)   : 750=0.04%, 1000=0.14%
  lat (msec)   : 2=0.63%, 4=1.06%, 10=97.61%, 20=0.49%, 50=0.03%
  lat (msec)   : 100=0.01%
  cpu          : usr=0.07%, sys=0.32%, ctx=541757, majf=0, minf=158
  IO depths    : 1=100.0%, 2=0.0%, 4=0.0%, 8=0.0%, 16=0.0%, 32=0.0%, >=64=0.0%
     submit    : 0=0.0%, 4=100.0%, 8=0.0%, 16=0.0%, 32=0.0%, 64=0.0%, >=64=0.0%
     complete  : 0=0.0%, 4=100.0%, 8=0.0%, 16=0.0%, 32=0.0%, 64=0.0%, >=64=0.0%
     issued rwts: total=0,536584,0,0 short=0,0,0,0 dropped=0,0,0,0
     latency   : target=0, window=0, percentile=100.00%, depth=1
 
Run status group 0 (all jobs):
  WRITE: bw=46.6MiB/s (48.8MB/s), 46.6MiB/s-46.6MiB/s (48.8MB/s-48.8MB/s), io=8384MiB (8791MB), run=180005-180005msec
 
Disk stats (read/write):
  nvme7n1: ios=0/554468, merge=0/64968, ticks=0/2994162, in_queue=2271908, util=100.00%
```

# GP3 Random Write
```bash
root@test-0:/# fio --directory=/pvc/gp3 --ioengine=psync --name rwrite --direct=1 --rw=randwrite --bs=16k --size=1G --numjobs=16 --time_based --runtime=180 --group_reporting --norandommap
```

```
rwrite: (g=0): rw=randwrite, bs=(R) 16.0KiB-16.0KiB, (W) 16.0KiB-16.0KiB, (T) 16.0KiB-16.0KiB, ioengine=psync, iodepth=1
Jobs: 16 (f=16): [w(16)][100.0%][w=46.9MiB/s][w=3001 IOPS][eta 00m:00s]
rwrite: (groupid=0, jobs=16): err= 0: pid=776: Fri May  7 04:36:19 2021
  write: IOPS=2997, BW=46.8MiB/s (49.1MB/s)(8432MiB/180005msec); 0 zone resets
    clat (usec): min=636, max=37031, avg=5335.60, stdev=729.33
     lat (usec): min=637, max=37032, avg=5336.11, stdev=729.29
    clat percentiles (usec):
     |  1.00th=[ 2999],  5.00th=[ 4948], 10.00th=[ 5080], 20.00th=[ 5211],
     | 30.00th=[ 5211], 40.00th=[ 5276], 50.00th=[ 5342], 60.00th=[ 5342],
     | 70.00th=[ 5407], 80.00th=[ 5473], 90.00th=[ 5538], 95.00th=[ 5669],
     | 99.00th=[ 7111], 99.50th=[ 9896], 99.90th=[13566], 99.95th=[16057],
     | 99.99th=[20841]
   bw (  KiB/s): min=37152, max=105586, per=99.99%, avg=47958.93, stdev=201.78, samples=5758
   iops        : min= 2322, max= 6599, avg=2996.91, stdev=12.61, samples=5758
  lat (usec)   : 750=0.01%, 1000=0.08%
  lat (msec)   : 2=0.60%, 4=0.72%, 10=98.15%, 20=0.43%, 50=0.01%
  cpu          : usr=0.07%, sys=0.34%, ctx=545183, majf=0, minf=170
  IO depths    : 1=100.0%, 2=0.0%, 4=0.0%, 8=0.0%, 16=0.0%, 32=0.0%, >=64=0.0%
     submit    : 0=0.0%, 4=100.0%, 8=0.0%, 16=0.0%, 32=0.0%, 64=0.0%, >=64=0.0%
     complete  : 0=0.0%, 4=100.0%, 8=0.0%, 16=0.0%, 32=0.0%, 64=0.0%, >=64=0.0%
     issued rwts: total=0,539616,0,0 short=0,0,0,0 dropped=0,0,0,0
     latency   : target=0, window=0, percentile=100.00%, depth=1
 
Run status group 0 (all jobs):
  WRITE: bw=46.8MiB/s (49.1MB/s), 46.8MiB/s-46.8MiB/s (49.1MB/s-49.1MB/s), io=8432MiB (8841MB), run=180005-180005msec
 
Disk stats (read/write):
  nvme8n1: ios=0/541468, merge=0/62440, ticks=0/2866164, in_queue=2148896, util=100.00%
```
