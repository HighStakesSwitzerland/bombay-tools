# Bombay State sync

## How to

### 1/
Get the wasm data https://highstakes.fra1.digitaloceanspaces.com/wasm.tar.lz4 and extract it to `~/.terra/data/wasm`

### 2/

In `config.toml` change

```
persistent_peers = "98efc6235550849fb04b50c890bf720c711a128b@144.2.71.66:30656" # make sure you have a cnx to our node

[statesync]
enable = true

rpc_servers = "terran.stakers.finance:30657,terran.stakers.finance:30657"
trust_height = the_choosed_block_height
trust_hash = "block_height"
trust_period = "168h0m0s"
```

Snapshots are taken every 20000 blocks, so set `trust_height` to the previous occurence of 20k (i.e. 7840000, 7820000 etc).

To get the `trust_hash` use the public lcd: https://bombay-lcd.terra.dev/blocks/7840000:

```
{
  "block_id": { 
    "hash":"4717A2C95C336A551CF908739DB9D29C818FBBCB126083D4DB8D98410FD9C843"
    ...
```

In `app.toml` set `halt-height` to the same block height. This is important or your node will corrupt the db after finishing the restore!

### 3/
Start it, it should download and apply the snapshot. This step takes a very long time and consumes a LOT of memory. 
If it fails, remove all `.terra/data` folder EXCEPT `wasm`, and restart it.

### 4/
Once the snapshot is restored, the node will shutdown because of `halt-height`. Change back this value to 0, add your seeds and persistent_peers, and you're good to go!
