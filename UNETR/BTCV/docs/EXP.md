# EXP

## Re-implement Results


```shell
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

The data loading function takes too much time: 
24 volumes, 13:45 min.
Note that there is another job running and consuming CPU.