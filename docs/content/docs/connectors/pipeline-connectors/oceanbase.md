---
title: "OceanBase"
weight: 6
type: docs
aliases:
- /connectors/pipeline-connectors/oceanbase
---
<!--
Licensed to the Apache Software Foundation (ASF) under one
or more contributor license agreements.  See the NOTICE file
distributed with this work for additional information
regarding copyright ownership.  The ASF licenses this file
to you under the Apache License, Version 2.0 (the
"License"); you may not use this file except in compliance
with the License.  You may obtain a copy of the License at

  http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing,
software distributed under the License is distributed on an
"AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY
KIND, either express or implied.  See the License for the
specific language governing permissions and limitations
under the License.
-->

# OceanBase Connector

OceanBase connector can be used as the *Data Sink* of the pipeline, and write data to [OceanBase](https://github.com/oceanbase/oceanbase). This document describes how to set up the OceanBase connector.

## What can the connector do?
* Create table automatically if not exist
* Schema change synchronization
* Data synchronization

## Example

The pipeline for reading data from MySQL and sink to OceanBase can be defined as follows:

```yaml
source:
  type: mysql
  hostname: mysql
  port: 3306
  username: mysqluser
  password: mysqlpw
  tables: mysql_2_oceanbase_test_17l13vc.\.*
  server-id: 5400-5404
  server-time-zone: UTC

sink:
  type: oceanbase
  url: jdbc:mysql://oceanbase:2881/test
  username: root@test
  password:

pipeline:
  name: MySQL to OceanBase Pipeline
  parallelism: 1
```

## Connector Options

<div class="highlight">
<table class="colwidths-auto docutils">
    <thead>
      <tr>
        <th>Option</th>
        <th>Required by Table API</th>
        <th>Default</th>
        <th>Type</th>
        <th>Description</th>
      </tr>
    </thead>
    <tbody>
    <tr>
      <td>url</td>
      <td>Yes</td>
      <td></td>
      <td>String</td>
      <td>JDBC url.</td>
    </tr>
    <tr>
      <td>username</td>
      <td>Yes</td>
      <td></td>
      <td>String</td>
      <td>The username.</td>
    </tr>
    <tr>
      <td>password</td>
      <td>Yes</td>
      <td></td>
      <td>String</td>
      <td>The password.</td>
    </tr>
    <tr>
      <td>schema-name</td>
      <td>Yes</td>
      <td></td>
      <td>String</td>
      <td>The schema name or database name.</td>
    </tr>
    <tr>
      <td>table-name</td>
      <td>Yes</td>
      <td></td>
      <td>String</td>
      <td>The table name.</td>
    </tr>
    <tr>
      <td>driver-class-name</td>
      <td>No</td>
      <td>com.mysql.cj.jdbc.Driver</td>
      <td>String</td>
      <td>The driver class name, use 'com.mysql.cj.jdbc.Driver' by default. If other value is set, you need to introduce the driver manually.</td>
    </tr>
    <tr>
      <td>druid-properties</td>
      <td>No</td>
      <td></td>
      <td>String</td>
      <td>Druid connection pool properties, multiple values are separated by semicolons.</td>
    </tr>
    <tr>
      <td>sync-write</td>
      <td>No</td>
      <td>false</td>
      <td>Boolean</td>
      <td>Whether to write data synchronously, will not use buffer if it's set to 'true'.</td>
    </tr>
    <tr>
      <td>buffer-flush.interval</td>
      <td>No</td>
      <td>1s</td>
      <td>Duration</td>
      <td>Buffer flush interval. Set '0' to disable scheduled flushing.</td>
    </tr>
    <tr>
      <td>buffer-flush.buffer-size</td>
      <td>No</td>
      <td>1000</td>
      <td>Integer</td>
      <td>Buffer size.</td>
    </tr>
    <tr>
      <td>max-retries</td>
      <td>No</td>
      <td>3</td>
      <td>Integer</td>
      <td>Max retry times on failure.</td>
    </tr>
    <tr>
      <td>memstore-check.enabled</td>
      <td>No</td>
      <td>true</td>
      <td>Boolean</td>
      <td>Whether enable memstore check.</td>
    </tr>
    <tr>
      <td>memstore-check.threshold</td>
      <td>No</td>
      <td>0.9</td>
      <td>Double</td>
      <td>Memstore usage threshold ratio relative to the limit value.</td>
    </tr>
    <tr>
      <td>memstore-check.interval</td>
      <td>No</td>
      <td>30s</td>
      <td>Duration</td>
      <td>Memstore check interval.</td>
    </tr>
    <tr>
      <td>partition.enabled</td>
      <td>No</td>
      <td>false</td>
      <td>Boolean</td>
      <td>Whether to enable partition calculation and flush records by partitions. Only works when 'sync-write' and 'direct-load.enabled' are 'false'.</td>
    </tr>
    <tr>
        <td>direct-load.enabled</td>
        <td>No</td>
        <td>false</td>
        <td>Boolean</td>
        <td>Whether to enable direct load. Note that direct load task requires the sink parallelism to be 1.</td>
    </tr>
    <tr>
        <td>direct-load.host</td>
        <td>No</td>
        <td></td>
        <td>String</td>
        <td>The hostname or IP address used in direct load task. Required when 'direct-load.enabled' is true.</td>
    </tr>
    <tr>
        <td>direct-load.port</td>
        <td>No</td>
        <td>2882</td>
        <td>Integer</td>
        <td>The rpc port used in direct load task. Required when 'direct-load.enabled' is true.</td>
    </tr>
    <tr>
        <td>direct-load.parallel</td>
        <td>No</td>
        <td>8</td>
        <td>Integer</td>
        <td>Parallelism of direct load task.</td>
    </tr>
    <tr>
        <td>direct-load.max-error-rows</td>
        <td>No</td>
        <td>0</td>
        <td>Long</td>
        <td>Maximum tolerable number of error rows of direct load task.</td>
    </tr>
    <tr>
        <td>direct-load.dup-action</td>
        <td>No</td>
        <td>REPLACE</td>
        <td>String</td>
        <td>Action when there is duplicated record of direct load task. Can be 'STOP_ON_DUP', 'REPLACE' or 'IGNORE'.</td>
    </tr>
    <tr>
        <td>direct-load.timeout</td>
        <td>No</td>
        <td>7d</td>
        <td>Duration</td>
        <td>Timeout for direct load task.</td>
    </tr>
    <tr>
        <td>direct-load.heartbeat-timeout</td>
        <td>No</td>
        <td>30s</td>
        <td>Duration</td>
        <td>Client heartbeat timeout in direct load task.</td>
    </tr>
</tbody>
</table>
</div>

## Usage Notes

* Currently only supports MySQL tenants of OceanBase

* Provides at-least-once semantics, exactly-once is not supported yet

* For creating table automatically
  * there is no partition key

* For schema change synchronization
  * Currently, only adding new columns and renaming columns are supported
  * the new column will always be added to the last position

* For data synchronization, the pipeline connector uses [OceanBase Sink Connector](https://github.com/oceanbase/flink-connector-oceanbase)
  to write data to OceanBase. You can see [sink documentation](https://github.com/oceanbase/flink-connector-oceanbase/blob/main/docs/sink/flink-connector-oceanbase.md)
  for how it works. 

## Data Type Mapping
<div class="wy-table-responsive">
<table class="colwidths-auto docutils">
    <thead>
      <tr>
        <th class="text-left">CDC type</th>
        <th class="text-left">OceanBase type under MySQL tenant</th>
        <th class="text-left" style="width:60%;">NOTE</th>
      </tr>
    </thead>
    <tbody>
    <tr>
      <td>TINYINT</td>
      <td>TINYINT</td>
      <td></td>
    </tr>
    <tr>
      <td>SMALLINT</td>
      <td>SMALLINT</td>
      <td></td>
    </tr>
    <tr>
      <td>INT</td>
      <td>INT</td>
      <td></td>
    </tr>
    <tr>
      <td>BIGINT</td>
      <td>BIGINT</td>
      <td></td>
    </tr>
    <tr>
      <td>FLOAT</td>
      <td>FLOAT</td>
      <td></td>
    </tr>
    <tr>
      <td>DOUBLE</td>
      <td>DOUBLE</td>
      <td></td>
    </tr>
    <tr>
      <td>DECIMAL(p, s)</td>
      <td>DECIMAL(p, s)</td>
      <td></td>
    </tr>
    <tr>
      <td>BOOLEAN</td>
      <td>BOOLEAN</td>
      <td></td>
    </tr>
    <tr>
      <td>DATE</td>
      <td>DATE</td>
      <td></td>
    </tr>
    <tr>
      <td>TIMESTAMP</td>
      <td>TIMESTAMP</td>
      <td></td>
    </tr>
    <tr>
      <td>TIMESTAMP_LTZ</td>
      <td>TIMESTAMP</td>
      <td></td>
    </tr>
    <tr>
      <td>CHAR(n) where n <= 256</td>
      <td>CHAR(n)</td>
      <td></td>
    </tr>
    <tr>
      <td>CHAR(n) where n > 256</td>
      <td>VARCHAR(n)</td>
      <td></td>
    </tr>
    <tr>
      <td>VARCHAR(n)</td>
      <td>VARCHAR(n)</td>
      <td></td>
    </tr>
    </tbody>
</table>
</div>

{{< top >}}