# Tarantool with Centralized configuration management using etcd

## Quick start

### Start etcd

```shell
etcd \
  --listen-client-urls=http://0.0.0.0:2379 \
  --advertise-client-urls=http://localhost:2379 \
  --enable-v2=true \
  --log-outputs=stderr
```

### Setup etcd

```shell
etcdctl user add root:secret
etcdctl user grant-role root root
etcdctl role add config_manager
etcdctl role grant-permission config_manager --prefix=true readwrite /etcd-config/app
etcdctl user add manager:password
etcdctl user grant-role manager config_manager
etcdctl auth enable
```

### Publish configuration into etcd

```shell
tt cluster publish "http://manager:password@localhost:2379/etcd-config/app" ./etcd-config/cluster-config.yml
```

### Start Tarantool

```shell
tt build etcd-config
tt start etcd-config
```