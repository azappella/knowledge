# ipfs

20 USD/year for 100 GB

Are clustered IPFS nodes really the same as a private IPFS swarm?

They are separate features/functionality.

- With private networks each node specifies which other nodes it will connect to. Nodes in that network don't respond to communications from nodes outside that network.
- With ipfs-cluster you use a leader-based consensus algorithm to coordinate storage of a pinset -- distributing the set of data across the participating nodes based on whichever pattern you prefer

You could use these features together -- using ipfs-cluster to spread a pinset across a private network of nodes -- but they are completely separate features. They do not rely on each other. Support for private networks is functionality implemented within the core (go-ipfs) code base. ipfs-cluster is its own separate code base.

## private networks

- [how to create private networks thread](https://discuss.ipfs.io/t/how-to-create-a-private-network-of-ipfs/339/9)
- [ipfs experimental features](https://github.com/ipfs/go-ipfs/blob/master/docs/experimental-features.md#private-networks)
- [ipfs-driven google drive](https://discuss.ipfs.io/t/ipfs-driven-equivalent-of-google-drive/824)
