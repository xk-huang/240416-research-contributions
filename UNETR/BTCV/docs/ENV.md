# ENV

## Docker

```shell
docker build -t unetr:240416 -f docker/dockerfile .

XK_BS=/data3/xiaoke
XK_WS=$XK_BS/code/240416-research-contributions/UNETR/BTCV
cd $XK_WS

docker run -td \
--gpus all \
--ipc=host \
--network host \
--ulimit memlock=-1 --ulimit stack=67108864 \
--user $(id -u ${USER}):$(id -g ${USER}) \
-v /etc/passwd:/etc/passwd:ro -v /etc/group:/etc/group:ro -v /etc/shadow:/etc/shadow:ro \
--group-add $(getent group docker | cut -d: -f3) --group-add $(getent group sudo | cut -d: -f3) \
-e HOME=$HOME -e USER=$USER \
-v $HOME:$HOME \
-v $XK_BS:$XK_BS \
-w $(pwd) \
--name 240416-unetr-xiaoke \
unetr:240416

docker exec -it 240416-unetr-xiaoke bash
```


## Restore Workspace

First check the storage and make workspace:
```shell
df -h | grep data
```

Prepare files:
```shell
XK_BS=/data3/xiaoke
XK_WS=$XK_BS/code/240416-research-contributions/UNETR/BTCV

mkdir -p $XK_BS/{code,data}

git clone git@github.com:xk-huang/240416-research-contributions.git $XK_BS/code/240416-research-contributions
rsync -avP /HDD_data_storage_2u_1/xiaoke/data/unetr_btcv_data/unetr_btcv $XK_BS/data/unetr_btcv_data/

ln -s $XK_BS/data/unetr_btcv_data/unetr_btcv $XK_BS/code/240416-research-contributions/UNETR/BTCV/dataset/btcv
```

Setup env:
```
docker build -t painter:240409 -f docker/dockerfile .
```