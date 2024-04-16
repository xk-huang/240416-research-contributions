# ENV

## Docker

```shell
docker build -t unetr:240416 -f docker/dockerfile .

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
-v /data3/xiaoke/:/data3/xiaoke/ \
-w $(pwd) \
--name 240416-unetr-xiaoke \
unetr:240416

docker exec -it 240416-unetr-xiaoke bash
```