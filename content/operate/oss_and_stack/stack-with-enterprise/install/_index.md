---
Title: Install and upgrade modules
alwaysopen: false
categories:
- docs
- operate
- stack
description: null
linkTitle: Install and upgrade modules
weight: 4
---

Several modules, which provide Redis Stack features, come packaged with [Redis Enterprise]({{< relref "/operate/rs" >}}). However, if you want to use additional modules or upgrade a module to a more recent version, you need to:

1. [Install a module package]({{< relref "/operate/oss_and_stack/stack-with-enterprise/install/add-module-to-cluster" >}}) on the cluster.
1. [Enable a module]({{< relref "/operate/oss_and_stack/stack-with-enterprise/install/add-module-to-database" >}}) for a new database or [upgrade a module]({{< relref "/operate/oss_and_stack/stack-with-enterprise/install/upgrade-module" >}}) in an existing database.

For custom modules not packaged and certified by Redis, [package them with the RAMP utility]({{< relref "/operate/oss_and_stack/stack-with-enterprise/install/packaging-modules" >}}) before installation on a Redis Enterprise cluster.