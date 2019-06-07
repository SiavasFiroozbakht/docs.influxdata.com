---
title: Configure InfluxDB Enterprise meta modes
description: Covers the InfluxDB Enterprise meta node configuration settings and environmental variables
menu:
  enterprise_influxdb_1_7:
    name: Configure meta nodes
    weight: 30
    parent: Administration
---

* [Meta node configuration settings](#meta-node-configurations)
  * [Global options](#global-options)
  * [Enterprise license `[enterprise]`](#enterprise)
  * [Meta node `[meta]`](#meta)

## Meta node configuration settings

### Global options

#### `reporting-disabled = false`

InfluxData, the company, relies on reported data from running nodes primarily to
track the adoption rates of different InfluxDB versions.
These data help InfluxData support the continuing development of InfluxDB.

The `reporting-disabled` option toggles the reporting of data every 24 hours to
`usage.influxdata.com`.
Each report includes a randomly-generated identifier, OS, architecture,
InfluxDB version, and the number of databases, measurements, and unique series.
To disable reporting, set this option to `true`.

> **Note:** No data from user databases are ever transmitted.

#### `bind-address = ""`

This setting is not intended for use.
It will be removed in future versions.

#### `hostname = ""`

The hostname of the [meta node](/enterprise_influxdb/v1.7/concepts/glossary/#meta-node).
This must be resolvable and reachable by all other members of the cluster.

Environment variable: `INFLUXDB_HOSTNAME`

-----

### Enterprise license settings

#### `[enterprise]`

The `[enterprise]` section contains the parameters for the meta node's
registration with the [InfluxDB Enterprise License Portal](https://portal.influxdata.com/).

#### `license-key = ""`

The license key created for you on [InfluxPortal](https://portal.influxdata.com).
The meta node transmits the license key to
[portal.influxdata.com](https://portal.influxdata.com) over port 80 or port 443
and receives a temporary JSON license file in return.
The server caches the license file locally.
You must use the [`license-path` setting](#license-path) if your server cannot
communicate with [https://portal.influxdata.com](https://portal.influxdata.com).

Use the same key for all nodes in the same cluster.
<dt>The `license-key` and `license-path` settings are mutually exclusive and one must remain set to the empty string.
</dt>

InfluxData recommends performing rolling restarts on the nodes after the license key update.
Restart one meta node or data node service at a time and wait for it to come back up successfully.
The cluster should remain unaffected as long as only one node is restarting at a
time as long as there are two or more data nodes.

Environment variable: `INFLUXDB_ENTERPRISE_LICENSE_KEY`

#### `license-path = ""`

The local path to the permanent JSON license file that you received from InfluxData
for instances that do not have access to the Internet.
To obtain a license file, contact [sales@influxdb.com](mailto:sales@influxdb.com).

The license file must be saved on every server in the cluster, including meta nodes
and data nodes.
The file contains the JSON-formatted license, and must be readable by the `influxdb` user.
Each server in the cluster independently verifies its license.

<dt>
The `license-key` and `license-path` settings are mutually exclusive and one must remain set to the empty string.
</dt>

InfluxData recommends performing rolling restarts on the nodes after the
license file update.
Restart one meta node or data node service at a time and wait for it to come back
up successfully.
The cluster should remain unaffected as long as only one node is restarting at a
time as long as there are two or more data nodes.

Environment variable: `INFLUXDB_ENTERPRISE_LICENSE_PATH`

-----

### Meta node settings

#### `[meta]`

#### `dir = "/var/lib/influxdb/meta"`

The directory where cluster meta data is stored.

Environment variable: `INFLUXDB_META_DIR`

#### `bind-address = ":8089"`

The bind address(port) for meta node communication.
For simplicity, InfluxData recommends using the same port on all meta nodes,
but this is not necessary.

Environment variable: `INFLUXDB_META_BIND_ADDRESS`

#### `http-bind-address = ":8091"`

The default address to bind the API to.

Environment variable: `INFLUXDB_META_HTTP_BIND_ADDRESS`

#### `https-enabled = false`

Determines whether meta nodes use HTTPS to communicate with each other.

Environment variable: `INFLUXDB_META_HTTPS_ENABLED`

#### `https-certificate = ""`

The SSL certificate to use when HTTPS is enabled.  
The certificate should be a PEM-encoded bundle of the certificate and key.  
If it is just the certificate, a key must be specified in https-private-key.

Environment variable: `INFLUXDB_META_HTTPS_CERTIFICATE`

#### `https-private-key = ""`

Use a separate private key location.

Environment variable: `INFLUXDB_META_HTTPS_PRIVATE_KEY`

#### `https-insecure-tls = false`

Whether meta nodes will skip certificate validation communicating with each other over HTTPS.
This is useful when testing with self-signed certificates.

Environment variable: `INFLUXDB_META_HTTPS_INSECURE_TLS`

#### `auth-enabled = false`

Set to `true` to enable authentication.
Meta nodes support JWT authentication and Basic authentication.
For JWT authentication, see also the [`shared-secret`](#shared-secret) and
[`internal-shared-secret`](#internal-shared-secret) configuration settings.

If set to `true`, also set the [`meta-auth-enabled` option](/enterprise_influxdb/v1.7/administration/config-data-nodes#meta-auth-enabled-false) to `true` in the `[meta]` section of the data node configuration file.

Environment variable: `INFLUXDB_META_AUTH_ENABLED`

####  `http-bind-address = ":8091"`

The port used by the [`influxd-ctl` tool](/enterprise_influxdb/v1.7/administration/cluster-commands/) and by data nodes to access the Meta API.
For simplicity, InfluxData recommends using the same port on all meta nodes, but this
is not necessary.

Environment variable: `INFLUXDB_META_HTTP_BIND_ADDRESS`

#### `https-enabled = false`

Determines whether meta nodes use HTTPS to communicate with each other.

#### `https-certificate = ""`

The SSL certificate to use when HTTPS is enabled.  
The certificate should be a PEM-encoded bundle of the certificate and key.  
If it is just the certificate, a key must be specified in https-private-key.

#### `https-private-key = ""`

Use a separate private key location.

#### `https-insecure-tls = false`

Whether meta nodes will skip certificate validation communicating with each other over HTTPS.
This is useful when testing with self-signed certificates.

#### `https-enabled = false`

Set to `true` to if using HTTPS over the `8091` API port.
Currently, the `8089` and `8088` ports do not support TLS.

Environment variable: `INFLUXDB_META_HTTPS_ENABLED`

#### `https-certificate = ""`

The path of the certificate file.
This is required if [`https-enabled`](#https-enabled-false) is set to `true`.

Environment variable: `INFLUXDB_META_HTTPS_CERTIFICATE`

#### `https-private-key = ""`

The path of the private key file.

Environment variable: `INFLUXDB_META_HTTPS_PRIVATE_KEY`

#### `https-insecure-tls = false`

Set to `true` to allow insecure HTTPS connections to meta nodes.
Use this setting when testing with self-signed certificates.

Environment variable: `INFLUXDB_META_HTTPS_INSECURE_TLS`

#### `gossip-frequency = "5s"`

The frequency at which meta nodes communicate the cluster membership state.

Environment variable: `INFLUXDB_META_GOSSIP_FREQUENCY`

#### `announcement-expiration = "30s"`

The rate at which the results of `influxd-ctl show` are updated when a meta
node leaves the cluster.
Note that in version 1.0, configuring this setting provides no change from
the user's perspective.

Environment variable: `INFLUXDB_META_ANNOUNCEMENT_EXPIRATION`

#### `retention-autocreate = true`

Automatically creates a default [retention policy](/influxdb/v1.7/concepts/glossary/#retention-policy-rp) (RP) when the system creates a database.
The default RP (`autogen`) has an infinite duration, a shard group duration of seven days, and a replication factor set to the number of data nodes in the cluster.
The system targets the `autogen` RP when a write or query does not specify an RP.
Set this option to `false` to prevent the system from creating the `autogen` RP when the system creates a database.

Environment variable: `INFLUXDB_META_RETENTION_AUTOCREATE`

#### `election-timeout = "1s"`

The duration a Raft candidate spends in the candidate state without a leader
before it starts an election.
The election timeout is slightly randomized on each Raft node each time it is called.
An additional jitter is added to the `election-timeout` duration of between zero and the `election-timeout`.
The default setting should work for most systems.

Environment variable: `INFLUXDB_META_ELECTION_TIMEOUT`

#### `heartbeat-timeout = "1s"`

The heartbeat timeout is the amount of time a Raft follower remains in the follower
state without a leader before it starts an election.
Clusters with high latency between nodes may want to increase this parameter to
avoid unnecessary Raft elections.

Environment variable: `INFLUXDB_META_HEARTBEAT_TIMEOUT`

#### `leader-lease-timeout = "500ms"`

The leader lease timeout is the amount of time a Raft leader will remain leader
 if it does not hear from a majority of nodes.
After the timeout the leader steps down to the follower state.
Clusters with high latency between nodes may want to increase this parameter to
 avoid unnecessary Raft elections.

Environment variable: `INFLUXDB_META_LEADER_LEASE_TIMEOUT`

#### `consensus-timeout = "30s`"

Environment variable: `INFLUXDB_META_CONSENSUS_TIMEOUT`

#### `commit-timeout = "50ms"`

The commit timeout is the amount of time a Raft node will tolerate between
commands before issuing a heartbeat to tell the leader it is alive.
The default setting should work for most systems.

Environment variable: `INFLUXDB_META_COMMIT_TIMEOUT`

#### `cluster-tracing = false`

Cluster tracing toggles the logging of Raft logs on Raft nodes.
Enable this setting when debugging Raft consensus issues.

Environment variable: `INFLUXDB_META_CLUSTER_TRACING`

#### `logging-enabled = true`

Meta logging toggles the logging of messages from the meta service.

Environment variable: `INFLUXDB_META_LOGGING_ENABLED`

#### `pprof-enabled = true`

Enables the `/debug/pprof` endpoint for troubleshooting.
To disable, set the value to `false`.

Environment variable: `INFLUXDB_META_PPROF_ENABLED`

#### `lease-duration = "1m0s"`

The default duration of the leases that data nodes acquire from the meta nodes.
Leases automatically expire after the `lease-duration` is met.

Leases ensure that only one data node is running something at a given time.
For example, [Continuous Queries](/influxdb/v1.7/concepts/glossary/#continuous-query-cq)
(CQs) use a lease so that all data nodes aren't running the same CQs at once.

For more details about `lease-duration` and its impact on continuous queries, see
[Configuration and operational considerations on a cluster](enterprise_influxdb/latest/features/clustering-features/#configuration-and-operational-considerations-on-a-cluster).

Environment variable: `INFLUXDB_META_LEASE_DURATION`

#### `shared-secret = ""`

The shared secret to be used by the public API for creating custom JWT authentication.
If you use this setting, set [`auth-enabled`](#auth-enabled-false) to `true`.

Environment variable: `INFLUXDB_META_SHARED_SECRET`

#### `internal-shared-secret = ""`

The shared secret used by the internal API for JWT authentication for
inter-node communication within the cluster.
Set this to a long pass phrase.
This value must be the same value as the
[`[meta] meta-internal-shared-secret`](/enterprise_influxdb/v1.7/administration/config-data-nodes#meta-internal-shared-secret) in the data node configuration file.
To use this option, set [`auth-enabled`](#auth-enabled-false) to `true`.

Environment variable: `INFLUXDB_META_INTERNAL_SHARED_SECRET`
