# Cấu hình redis cluster

## Bước 1: Download Docker image
```sh
docker pull redis
```
## Bước 2: Tạo file Redis.conf
Tạo 6 thư mục, mỗi thư mục chứa 1 file redis.config, được sử dụng để tạo container redis
```sh
mkdir 7000 7001 7002 7003 7004 7005
```

Nội dung file redis.config
```sh
port <port-redis>  
cluster-enabled yes  
cluster-config-file nodes.conf  
cluster-node-timeout <port-cluster>  
bind <server-IP>
```
Ví dụ: 
```sh
port 7000  
cluster-enabled yes  
cluster-config-file nodes.conf  
cluster-node-timeout 5000  
bind localhost
```

## Bước 3: Start Docker containers
Sử dụng image đã pull từ bước 1, tạo ra các container

```sh
<folder-path>\7000\redis.conf:/usr/local/etc/redis/redis.conf -d --net=host --name <container-name>  redis redis-server /usr/local/etc/redis/redis.conf
```

Ví dụ:
```sh
docker run -v D:\home\Study\Redis\DemoCluster\7000\redis.conf:/usr/local/etc/redis/redis.conf -d --net=host --name myredis-7000 redis redis-server /usr/local/etc/redis/redis.conf

docker run -v D:\home\Study\Redis\DemoCluster\7001\redis.conf:/usr/local/etc/redis/redis.conf -d --net=host --name myredis-7001 redis redis-server /usr/local/etc/redis/redis.conf

docker run -v D:\home\Study\Redis\DemoCluster\7002\redis.conf:/usr/local/etc/redis/redis.conf -d --net=host --name myredis-7002 redis redis-server /usr/local/etc/redis/redis.conf

docker run -v D:\home\Study\Redis\DemoCluster\7003\redis.conf:/usr/local/etc/redis/redis.conf -d --net=host --name myredis-7003 redis redis-server /usr/local/etc/redis/redis.conf

docker run -v D:\home\Study\Redis\DemoCluster\7004\redis.conf:/usr/local/etc/redis/redis.conf -d --net=host --name myredis-7004 redis redis-server /usr/local/etc/redis/redis.conf

docker run -v D:\home\Study\Redis\DemoCluster\7005\redis.conf:/usr/local/etc/redis/redis.conf -d --net=host --name myredis-7005 redis redis-server /usr/local/etc/redis/redis.conf

docker run -v D:\home\Study\Redis\DemoCluster\7006\redis.conf:/usr/local/etc/redis/redis.conf -d --net=host --name myredis-7006 redis redis-server /usr/local/etc/redis/redis.conf
```

## Bước 4: Tạo cluster sử dụng redis nodes
Truy cập vào màn hình command line của 1 redis
```sh
docker exec -it <container-id> sh
```
Truy cập redis-cli với port khác mặc định (VD: port 7000)
```sh
redis-cli -p 7000
```

Tạo cluster
```sh
redis-cli --cluster create <server-IP>:7000 <server-IP>:7001 <server-IP>:7002 <server-IP>:7003 <server-IP>:7004 <server-IP>:7005 --cluster-replicas 1
```

vD 
```sh
redis-cli --cluster create localhost:7000 localhost:7001 localhost:7002 localhost:7003 localhost:7004 localhost:7005 --cluster-replicas 1
```
Chú ý:<br/>
*`<server-IP`> sẽ là thông tin trong file redis.conf file <br/>
--cluster-replicas 1 sẽ tạo ra 3 nodes là  master nodes và 3 nodes cuối là slave nodes.*


## Link tài liệu tham khảo
https://www.dltlabs.com/blog/how-to-setup-configure-a-redis-cluster-easily-573120

## Một số link hỗ trợ lỗi khi setup
https://stackoverflow.com/questions/31676155/docker-error-response-from-daemon-conflict-already-in-use-by-container