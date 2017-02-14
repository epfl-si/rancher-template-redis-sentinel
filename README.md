Redis-Sentinel Template
=======================

This repository contains a Rancher Template used in [the EPFL rancher-catalog](https://github.com/epfl-idevelop/rancher-catalog).

This template runs a [redis](http://redis.io) instance in HA using Sentinel.

The redis image is taken from the Docker Hub Library (AKA "Official" image). The configuration is generated using [a custom confd image with shared volume](https://github.com/epfl-idevelop/container-redis-conf).

## Requirements
To avoid losing multiple Sentinel and Redis Nodes at once, the catalog prohibits starting multiple instances on the same host. This means that to have a 3-node setup of Sentinel (or redis), 3 Hosts are required. A single node can run multiple instance of the Template (thus can have multiple redis running independently). Sentinel and Redis are not mutually exclusive on the same host, so for a 3-node sentinel and a 4-node redis, a minimum of 4 Hosts is required.

This Template does not configure the underlying Host, so redis will complain (write Warnings to the container's log) about host configuration to avoid the issues described in the Warning message, please configure the Host correctly.

## Usage Recommendations

 - Once the Template is deployed in rancher, each service can be scaled-up independently.
 - Sentinel should always be scaled to an odd number of containers to have proper Quorum.
 - If a Sentinel container is destroyed, the other Sentinel must be reconfigured to `forget` it, as documented in the `Adding or removing Sentinels` section of the [Sentinel Documentation](https://redis.io/topics/sentinel)
 - Restarting a sentinel with the same configuration in another container (i.e. rancher upgrade process) *should* be transparent but is *untested*
 - The redis cluster is automatically maintained by Sentinel, so Master-Slave replications should always follow but *not all cases were tested*

(c) All rights reserved. ECOLE POLYTECHNIQUE FEDERALE DE LAUSANNE, Switzerland, VPSI, 2017
