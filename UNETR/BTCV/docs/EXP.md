# EXP

## Re-implement Results

https://github.com/Project-MONAI/research-contributions/tree/main/UNETR/BTCV

```shell
XK_BS=/data3/xiaoke
XK_WS=$XK_BS/code/240416-research-contributions/UNETR/BTCV

CUDA_VISIBLE_DEVICES=2 \
python main.py \
--feature_size=32 \
--batch_size=1 \
--logdir=unetr_test \
--optim_lr=1e-4 \
--lrschedule=warmup_cosine \
--infer_overlap=0.5 \
--save_checkpoint \
--data_dir=dataset/btcv
```

For distributed training, turn on `--distributed` flag and linearly inscrease the learning basedon `1e-4`.
```
CUDA_VISIBLE_DEVICES=4,5,6 \
python main.py \
--feature_size=32 \
--batch_size=1 \
--logdir=unetr_test \
--optim_lr=3e-4 \
--lrschedule=warmup_cosine \
--infer_overlap=0.5 \
--save_checkpoint \
--data_dir=dataset/btcv \
--distributed --use_normal_dataset
```


### Data Loading Time

The data loading function takes too much time: 

4u-9
24 volumes, 13:45 min, w/ other CPU extensive jobs.
It is observed that the CPU usage is not optimial, half of the CPUs are void even with other jobs.

4u-3
Terribly Slow.

This is because the SSDs are SATA, while the faster storage on 4u-9 is NVME.
Check the spec by: `lsblk -o NAME,FSTYPE,LABEL,MOUNTPOINT,SIZE,MODEL`.

> NVMe SSDs have a queue depth of 64,000, while SATA can only support 32 I/O requests in a queue at any time.
> NVME goes through both AHCI and PCIE, 3Gb/s; while SATA goes AHCI, maximum 500Mb/s

#### Benchmark IO

The command for benchmark speed:
```shell
df -h | grep data  # Find the device file

DEVICE_FILE=
sudo dd if=$DEVICE_FILE of=/dev/null bs=4k
# C^+C to stop
```

Results:
There are other jobs doing IO, so 
```

4u-9:/dev/nvme1n1 
22054019072 bytes (22 GB, 21 GiB) copied, 10.1385 s, 2.2 GB/s
4u-9:/dev/sdb1 
4036292608 bytes (4.0 GB, 3.8 GiB) copied, 13.5244 s, 298 MB/s
4u-9:/dev/sda1
8091136000 bytes (8.1 GB, 7.5 GiB) copied, 16.1587 s, 501 MB/s

4u-3:/dev/{sdb1,sda1,sdc1} 5697032192 bytes (5.7 GB, 5.3 GiB) copied, 18.69 s, 305 MB/s
```