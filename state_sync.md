## [![Typing SVG](https://readme-typing-svg.herokuapp.com?weight=900&size=30&pause=1000&color=A7F79D&background=FF942B00&center=%D0%9B%D0%9E%D0%96%D0%AC&vCenter=%D0%9B%D0%9E%D0%96%D0%AC&repeat=%D0%B8%D1%81%D1%82%D0%B8%D0%BD%D0%BD%D1%8B%D0%B9&width=435&lines=STATE+SYNC+GNST+PLANQ)](https://git.io/typing-svg)
```
peers="1a1785bf66f47a2eff058fe770be6b6b1b694400@38.242.148.96:27656"; \
sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" $HOME/.planqd/config/config.toml

SNAP_RPC="https://planq-rpc.gnst.systemd.run"; \
LATEST_HEIGHT=$(curl -s $SNAP_RPC/block | jq -r .result.block.header.height); \
BLOCK_HEIGHT=$((LATEST_HEIGHT - 500)); \
TRUST_HASH=$(curl -s "$SNAP_RPC/block?height=$BLOCK_HEIGHT" | jq -r .result.block_id.hash); \
echo $LATEST_HEIGHT $BLOCK_HEIGHT $TRUST_HASH

sed -i -E "s|^(enable[[:space:]]+=[[:space:]]+).*$|\1true| ; \
s|^(rpc_servers[[:space:]]+=[[:space:]]+).*$|\1\"$SNAP_RPC,$SNAP_RPC\"| ; \
s|^(trust_height[[:space:]]+=[[:space:]]+).*$|\1$BLOCK_HEIGHT| ; \
s|^(trust_hash[[:space:]]+=[[:space:]]+).*$|\1\"$TRUST_HASH\"|" ~/.planqd/config/config.toml



sudo systemctl stop planqd && \
planqd tendermint unsafe-reset-all --home $HOME/.planqd && \
sudo systemctl restart planqd
```
***