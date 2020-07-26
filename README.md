ckb-rosetta-docker
==================

> Coinbase Rosetta server docker, forked from [ckb-rich-node](https://github.com/ququzone/ckb-rich-node)

## Usage

### Local build

```bash
docker build -t ckb-rosetta-docker .
docker run --name ckb-rosetta-docker -d -p 8117:8117 -v "$PWD/data":/data ckb-rosetta-docker
```

### Pull from DockerHub

```
docker pull nervos/ckb-rosetta-docker
docker run --name ckb-rosetta-docker -d -p 8117:8117 -v "$PWD/data":/data nervos/ckb-rosetta-docker
```

### CKB node RPC

```
echo '{
    "id": 2,
    "jsonrpc": "2.0",
    "method": "get_blockchain_info",
    "params": []
}' \
| tr -d '\n' \
| curl -H 'content-type: application/json' -d @- \
http://localhost:8117/rpc
```

More RPC method can be refer [ckb rpc docs](https://github.com/nervosnetwork/ckb/blob/master/rpc/README.md)

### CKB indexer RPC

```
echo '{
    "id": 2,
    "jsonrpc": "2.0",
    "method": "get_cells",
    "params": [
        {
            "script": {
                "code_hash": "0x9bd7e06f3ecf4be0f2fcd2188b23f1b9fcc88e5d4b65a8637b17723bbda3cce8",
                "hash_type": "type",
                "args": "0x8211f1b938a107cd53b6302cc752a6fc3965638d"
            },
            "script_type": "lock"
        },
        "asc",
        "0x64"
    ]
}' \
| tr -d '\n' \
| curl -H 'content-type: application/json' -d @- \
http://localhost:8117/indexer
```

More RPC method can be refer [ckb-indexer](https://github.com/quake/ckb-indexer)

### Coinbase Rosetta Endpoint

```
http://localhost:8117/rosetta
```

Example code can be refer [ckb-rosetta-sdk](https://github.com/nervosnetwork/ckb-rosetta-sdk/blob/master/client/example.go)

### Check by rosetta-cli

- Install rosetta-cli

```
go get github.com/coinbase/rosetta-cli
```

- Check

```
docker run --name ckb-rosetta-docker -d -p 8117:8117 -v "$PWD/data":/data nervos/ckb-rosetta-docker
rosetta-cli check --server-url=http://localhost:8117/rosetta --inactive-reconciliation-frequency=1 --lookup-balance-by-block=false --inactive-reconciliation-concurrency=64
```

