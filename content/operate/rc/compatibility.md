---
Title: Redis Cloud compatibility with open source Redis
alwaysopen: false
categories:
- docs
- operate
- rc
description: Redis Cloud compatibility with open source Redis.
linkTitle: Open source compatibility
weight: 90
---

Both [Redis Enterprise Software]({{< relref "/operate/rs" >}}) and Redis Cloud are compatible with open source
Redis (OSS Redis). 

{{< embed-md "rc-rs-oss-compatibility.md"  >}}

## RESP compatibility

Redis Enterprise Software and Redis Cloud support RESP2 and RESP3. In Redis Cloud, you can choose between RESP2 and RESP3 when you [create a database]({{< relref "/operate/rc/databases/create-database" >}}) and you can change it when you [edit a database]({{< relref "/operate/rc/databases/view-edit-database" >}}). For more information about the different RESP versions, see the [Redis serialization protocol specification]({{< relref "/develop/reference/protocol-spec" >}}#resp-versions).

## Compatibility with open source Redis Cluster API

Redis Cloud supports [Redis OSS Cluster API]({{< relref "/operate/rc/databases/create-database#oss-cluster-api" >}}) on Flexible subscriptions if it is enabled for a database. Review [Redis OSS Cluster API architecture]({{< relref "/operate/rs/clusters/optimize/oss-cluster-api" >}}) to determine if you should enable this feature for your database.