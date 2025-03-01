# 基本概念

本节列出有关 Elasticsearch 涉及的专有名词及术语。

- 节点（node）

    数据节点：存储索引数据的节点，主要对文档进行增删改查、聚合等操作。

    专有主节点：对集群进行操作，例如创建或删除索引，跟踪哪些节点是集群的一部分，并决定哪些分片分配给相关的节点。稳定的主节点对集群的健康非常重要，默认情况下集群中的任一节点都可能被选为主节点。

    协调节点：分担数据节点的CPU开销，从而提高处理性能和服务稳定性。

- 索引（Index）

    用于存储Elasticsearch的数据，是一个或多个分片分组在一起的逻辑空间。

- 分片（Shard）

    索引可以存储数据量超过 1 个节点硬件限制的数据。为满足这样的需求，Elasticsearch 提供了一个能力，将一个索引拆分为多个，称为 Shard。
    当您创建一个索引时，您可以根据实际情况指定 Shard 的数量。每个 Shard 托管在集群中的任意一个节点中，且每个 Shard 本身是一个独立的、全功能的“索引”。
    Shard 的数量只能在创建索引前指定，且在索引创建成功后无法修改。

- 副本（replicas）

    replicas 是索引的备份，Elasticsearch 可以设置多个副本。写操作会先在主分片上完成，然后分发到副本分片上。
    因为索引的主分片和副本分片都可以对外提供查询服务，所以副本能够提升系统的高可用性和搜索时的并发性能。但如果副本太多，也会增加写操作时数据同步的负担。
