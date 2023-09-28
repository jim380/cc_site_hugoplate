---
title: "State Sync"
# meta title
meta_title: "State Sync"
# meta description
description: ""
# save as draft
draft: false
---

{{< toc >}}

<!-- https://www.textemoji.org -->

| **Chain** |                                   **URL**                                   |
| --------- | :-------------------------------------------------------------------------: |
| Umee      | {{< button label="´･ᴗ･`" link="http://66.206.5.194:26657" style="solid" >}} |

## Stop Existing Process

```bash
$ sudo systemctl stop umeed
```

## Reset Local Database

```bash
$ umeed tendermint unsafe-reset-all
```

## Set Block Interval

```bash
$ INTERVAL=1000 # modify as you see fit
```

## Set Necessary Parameters

```bash
$ RPC="http://23.92.76.110:26657"
$ LATEST_HEIGHT=$(curl -s $RPC/block | jq -r .result.block.header.height);
$ TRUST_HEIGHT=$(($LATEST_HEIGHT-$INTERVAL))
$ TRUST_HASH=$(curl -s "$RPC/block?height=$TRUST_HEIGHT" | jq -r .result.block_id.hash)
$ PEERS="ade4d8bc8cbe014af6ebdf3cb7b1e9ad36f412c0@seeds.polkachu.com:13656,ebc272824924ea1a27ea3183dd0b9ba713494f83@umee-mainnet-seed.autostake.com:26756,64cdbb45575825f764af7ff9d6c71471bc131f87@seed-node.mms.team:32656,88373a3bf385c20ef0b4040f924cd99848012535@seed-umee-01.stakeflow.io:26696"
```

Try our [tool](https://github.com/jim380/bootstrap-me) to get most updated, reachable peers.

## Populate the Values in `config.toml`

```bash
$ sed -i.bak -E "s|^(enable[[:space:]]+=[[:space:]]+).*$|\1true|; \
s|^(rpc_servers[[:space:]]+=[[:space:]]+).*$|\1\"$RPC,$RPC\"|; \
s|^(trust_height[[:space:]]+=[[:space:]]+).*$|\1$TRUST_HEIGHT|; \
s|^(trust_hash[[:space:]]+=[[:space:]]+).*$|\1\"$TRUST_HASH\"|" $HOME/.umee/config/config.toml
$ sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$PEERS\"/" $HOME/.umee/config/config.toml
```

## Restart the Process

```bash
$ sudo systemctl restart umeed
```
