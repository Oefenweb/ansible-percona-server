## percona-server

[![Build Status](https://travis-ci.org/Oefenweb/ansible-percona-server.svg?branch=master)](https://travis-ci.org/Oefenweb/ansible-percona-server) [![Ansible Galaxy](http://img.shields.io/badge/ansible--galaxy-percona--server-blue.svg)](https://galaxy.ansible.com/list#/roles/4882)

Set up a [percona-server](https://www.percona.com/software/mysql-database/percona-server) server in Debian-like systems.

#### Requirements

* `python-mysqldb` (will be installed)

#### Variables

##### General

* `percona_server_version`: [default: `5.5`]: Version to install
* `percona_server_root_password`: [default: `+eswuw9uthUteFreyAqu`]: Root password **Make sure to change!**
* `percona_server_manage_root_my_cnf`: [default: `true`]: Whether or not to manage `~root/.my.cnf`

* `percona_server_install`: [xtrabackup, percona-toolkit]: Additional packages to install

* `percona_server_socket`: [default: `/var/run/mysqld/mysqld.sock`]: On Unix platforms, this variable is the name of the socket file that is used for local client connections
* `percona_server_pid_file`: [default: `/var/run/mysqld/mysqld.pid`]: The path name of the process ID (PID) file
* `percona_server_port`: [default: `3306`]: The number of the port on which the server listens for TCP/IP connections
* `percona_server_datadir`: [default: `/var/lib/mysql`]: The path to the data directory

##### Logging

* `percona_server_general_log`: [default: `false`]: Whether the general query log is enabled
* `percona_server_general_log_file`: [default: `/var/log/mysql/mysql.log`]: The name of the general query log file
* `percona_server_log_error`: [default: `/var/log/mysql/error.log`]: The location of the error log, or empty if the server is writing error message to the standard error output
* `percona_server_log_queries_not_using_indexes`: [default: `false`]: Whether queries that do not use indexes are logged to the slow query log
* `percona_server_slow_query_log`: [default: `false`]: Whether slow queries should be logged. "Slow: is determined by the value of the `long_query_time` variable
* `percona_server_slow_query_log_file`: [default: `/var/log/mysql/mysql-slow.log`]: The name of the slow query log file

##### Dns

* `percona_server_skip_name_resolve`: [default: `true`]: Whether or not `mysqld` resolves host names when checking client connections

##### Connection

* `percona_server_max_connections`: [default: `150`]: The maximum permitted number of simultaneous client connections
* `percona_server_wait_timeout`: [default: `28800`]: The number of seconds the server waits for activity on a noninteractive connection before closing it
* `percona_server_interactive_timeout`: [default: `28800`]: The number of seconds the server waits for activity on an interactive connection before closing it

##### SQL mode

* `percona_server_sql_mode`: [default: `''`]: The SQL mode the server operates in. The default SQL mode is empty (no modes set)

##### Encoding

* `percona_server_character_set_server`: [default: `utf8`]: The server's default character set
* `percona_server_collation_server`: [default: `utf8_general_ci`]: The server's default collation
* `percona_server_skip_character_set_client_handshake`: [default: `true`]: Whether or not to ignore character set information sent by the client
* `percona_server_init_connect`: [default: `'SET collation_connection = utf8_general_ci; SET NAMES utf8;'`]: A string to be executed by the server for each client that connects

##### Engine

* `percona_server_default_storage_engine`: [default: `InnoDB`]: The default storage engine

##### MyISAM

* `percona_server_key_buffer_size`: [default: `32M`]: Index blocks for `MyISAM` tables are buffered and are shared by all threads. `key_buffer_size` is the size of the buffer used for index blocks
* `percona_server_isamchk_key_buffer_size`: [default: `percona_server_key_buffer_size`]: Same as above, but for `isamchk` section

##### Thread

* `percona_server_thread_stack`: [default: `256K`]: The stack size for each thread
* `percona_server_thread_cache_size`: [default: `8`]: How many threads the server should cache for reuse

##### Query cache

* `percona_server_query_cache_type`: [default: `1`]: Set the query cache type
* `percona_server_query_cache_limit`: [default: `1M`]: Do not cache results that are larger than this number of bytes
* `percona_server_query_cache_size`: [default: `16M`]: The amount of memory allocated for caching query results

##### Packet / string

* `percona_server_max_allowed_packet`: [default: `16M`]: The maximum size of one packet or any generated/intermediate string
* `percona_server_mysqldump_max_allowed_packet`: [default: `percona_server_max_allowed_packet`]: Same as above, but for `mysqldump` section
* `percona_server_group_concat_max_len`: [default: `1024`]: The maximum permitted result length in bytes for the `GROUP_CONCAT()` function

##### Temp / heap tables

* `percona_server_tmp_table_size`: [default: `16M`]: The maximum size of internal in-memory temporary tables
* `percona_server_max_heap_table_size`: [default: `16M`]: This variable sets the maximum size to which user-created `MEMORY` tables are permitted to grow

##### Open files

* `percona_server_open_files_limit`: [default: `1024`]: The number of files that the operating system permits `mysqld` to open
* `percona_server_innodb_open_files`: [default: `300`]: This variable is relevant only if you use multiple `InnoDB` tablespaces. It specifies the maximum number of `.ibd` files that MySQL can keep open at one time
* `percona_server_table_definition_cache`: [default: `400`]: The number of table definitions (from `.frm` files) that can be stored in the definition cache
* `percona_server_table_open_cache`: [default: `400`]: The number of open tables for all threads. Increasing this value increases the number of file descriptors that `mysqld` requires

##### Replication / binary log

* `percona_server_configure_replication`: [default: `false`]: Whether or not the configure replication (related settings, section below)
* `percona_server_server_id`: [required]: This option is common to both master and slave replication servers, and is used in replication to enable master and slave servers to identify themselves uniquely (e.g. `1`, `2`)

* `percona_server_report_host`: [default: `{{ inventory_hostname }}`]: The host name or IP address of the slave to be reported to the master during slave registration. This value appears in the output of `SHOW SLAVE HOSTS` on the master server

* `percona_server_read_only`: [default: `false`]: When the read_only system variable is enabled, the server permits no client updates except from users who have the `SUPER` privilege
* `percona_server_skip_slave_start`: [default: `false`]: Tells the slave server not to start the slave threads when the server starts

* `percona_server_expire_logs_days`: [default: `7`]: The number of days for automatic binary log file removal
* `percona_server_max_binlog_size`: [default: `1G`]: If a write to the binary log causes the current log file size to exceed the value of this variable, the server rotates the binary logs (closes the current file and opens the next one)

###### Master

* `percona_server_log_bin`: [optional, required for `master`]: Enable binary logging. The option value, if given, is the base name for the log sequence. The server creates binary log files in sequence by adding a numeric suffix to the base name (e.g. `mysql-bin`)
* `percona_server_log_bin_index`: [optional, required for `master`]: The index file for binary log file names (e.g. `mysql-bin.index`)
* `percona_server_sync_binlog`: [optional, required for `master`]: If the value of this variable is greater than `0`, the MySQL server synchronizes its binary log to disk (using `fdatasync()`) after every `sync_binlog` writes to the binary log (e.g. `1`)

###### Slave

* `percona_server_relay_log`: [optional, required for `slave`]: The option value, if given, is the base name for the log sequence. The server creates relay log files in sequence by adding a numeric suffix to the base name (e.g. `mysql-relay`)
* `percona_server_relay_log_index`: [optional, required for `slave`]: The index file for relay log file names (e.g. `mysql-relay.index`)
* `percona_server_sync_relay_log`: [optional, required for `slave`]: If the value of this variable is greater than 0``, the MySQL server synchronizes its relay log to disk (using `fdatasync()`) after every `sync_relay_log` events are written to the relay log (e.g. `1`)

##### InnoDB

* `percona_server_innodb_buffer_pool_size`: [default: `128M`]: The size in bytes of the buffer pool, the memory area where InnoDB caches table and index data
* `percona_server_innodb_additional_mem_pool_size`: [default: `8M`]: The size in bytes of a memory pool `InnoDB` uses to store data dictionary information and other internal data structures. The more tables you have in your application, the more memory you need to allocate here
* `percona_server_innodb_log_file_size: [default`: `50M`]: The size in bytes of each log file in a log group
* `percona_server_innodb_log_buffer_size: [default`: `8M`]: The size in bytes of the buffer that `InnoDB` uses to write to the log files on disk
* `percona_server_innodb_flush_log_at_trx_commit`: [default: `1`]: Controls the balance between strict `ACID` compliance for commit operations, and higher performance that is possible when commit-related I/O operations are rearranged and done in batches
* `percona_server_innodb_flush_method: [default`: `O_DIRECT`]: Defines the method used to flush data to the `InnoDB` data files and log files, which can affect I/O throughput
* `percona_server_innodb_thread_concurrency`: [default: `0`]: `InnoDB` tries to keep the number of operating system threads concurrently inside `InnoDB` less than or equal to the limit given by this variable
* `percona_server_innodb_write_io_threads`: [default: `4`]: The number of I/O threads for write operations in `InnoDB`
* `percona_server_innodb_read_io_threads`: [default: `4`]: The number of I/O threads for read operations in `InnoDB`
* `percona_server_innodb_io_capacity`: [default: `200`]: Sets an upper limit on the I/O activity performed by the `InnoDB` background tasks, such as flushing pages from the buffer pool and merging data from the change buffer

##### Metadata

* `percona_server_innodb_stats_on_metadata`: [default: `false`]: When this variable is enabled `InnoDB` updates statistics when metadata statements such as `SHOW TABLE STATUS` or `SHOW INDEX` are run, or when accessing the `INFORMATION_SCHEMA.TABLES` or `INFORMATION_SCHEMA.STATISTICS` tables

##### Percona only

* `percona_server_innodb_adaptive_flushing_method`: [default: `keep_average`]: Controls the way adaptive checkpointing is performed
* `percona_server_innodb_flush_neighbor_pages`: [default: `none`]: Specifies whether, when the dirty pages are flushed to the data file, the neighbor pages in the data file are also flushed at the same time or not
* `percona_server_log_warnings_suppress`: [default: `''`]: Is intended to provide a more general mechanism for disabling warnings
* `percona_server_query_response_time_stats`: [default: `true`]: Controls collection of query times

##### SSL

* `percona_server_ssl_map`: [default: `{}`]: SSL declarations
* `percona_server_ssl_map.key`: [required]: The identifier of the file (e.g. `ca-cert`)
* `percona_server_ssl_map.key.src`: [required]: The local path of the file to copy, can be absolute or relative (e.g. `../../../files/percona-server/etc/mysql/ca-cert.pem`)
* `percona_server_ssl_map.key.dest`: [required]: The remote path of the file to copy (e.g. `/etc/mysql/ca-cert.pem`)
* `percona_server_ssl_map.key.owner`: [optional, default `root`]: The name of the user that should own the file
* `percona_server_ssl_map.key.group`: [optional, default `mysql`]:The name of the group that should own the file
* `percona_server_ssl_map.key.mode`: [optional, default `0640`]: The mode of the file

##### Databases

* `percona_server_databases_present`: [default: `[]`]: Databases to `CREATE`
* `percona_server_databases_present.{n}.name`: [required]: The name of the database
* `percona_server_databases_present.{n}.collation`: [optional, default: `utf8_general_ci`]: The collation of the database
* `percona_server_databases_present.{n}.encoding`: [optional, default: `utf8`]: The character set of the database

* `percona_server_databases_absent`: [default: `[{name: test}]`]: Databases to `DROP`
* `percona_server_databases_absent.{n}.name`: [required]: The name of the database

##### Users

* `percona_server_users_present`: [default: `[]`]: Users to `CREATE`
* `percona_server_users_present.{n}.name`: [required]: The name of the user
* `percona_server_users_present.{n}.password`: [required]: The password of the user
* `percona_server_users_present.{n}.privs`: [required]: Privileges (e.g. `'test.*:ALL'`)
* `percona_server_users_present.{n}.hosts`: [optional, default: `percona_server_users_present_hosts`]: Hosts to `CREATE` privileges for (e.g. `%`)

* `percona_server_users_present_hosts`: [default: `[localhost]`]: Hosts to `CREATE` privileges for

* `percona_server_users_absent`: [default: `[{name: ''}]`]: Users to `DROP`
* `percona_server_users_absent.{n}.name`: [required]: The name of the user
* `percona_server_users_absent.{n}.hosts`: [optional, default: `percona_server_users_absent_hosts`]: Hosts to `DROP` privileges for (e.g. `%`)

* `percona_server_users_absent_hosts`: [default: `[{{ ansible_hostname }}, 127.0.0.1, localhost, ::1, %]`]: Hosts to `DROP` privileges for

##### Queries

* `percona_server_queries`: [default: `[]`]: Query declarations
* `percona_server_queries.{n}.database`: [required]: Name of the database to execute queries on
* `percona_server_queries.{n}.queries`: [default: `[]`]: A list of queries to execute

## Dependencies

None

## Recommended

* `percona-client` ([see](https://github.com/Oefenweb/ansible-percona-client), when `percona_server_manage_root_my_cnf` is `false`)
* `limits` ([see](https://github.com/Oefenweb/ansible-limits))

#### Example(s)

##### Simple

```yaml
---
- hosts: all
  roles:
    - percona-server
```

##### Configure databases and users

```yaml
---
- hosts: all
  roles:
    - percona-server
  vars:
    percona_server_databases_present:
      - name: ipsum
      - name: dolor

    percona_server_databases_absent:
      - name: sit
      - name: amet

    percona_server_users_present_hosts:
      - 'localhost'
      - '%'

    percona_server_users_present:
      - name: consectetur
        password: 'elit'
        privs:
          - 'ipsum.*:ALL'
          - 'dolor.*:ALL'
      - name: adipiscing
        password: 'lacus'
        privs:
          - 'ipsum.*:SELECT'
          - 'dolor.*:INSERT,UPDATE'
        hosts:
          - '%'

    percona_server_users_absent:
      - name: urna
      - name: vehicula
        hosts:
          - '%'
```

##### Configure SSL

```yaml
- hosts: all
  roles:
    - percona-server
  vars:
    percona_server_ssl_map:
      ca-cert:
        src: ../../../files/percona-server/etc/mysql/ca-cert.pem
        dest: /etc/mysql/ca-cert.pem
      client-cert:
        src: ../../../files/percona-server/etc/mysql/client-cert.pem
        dest: /etc/mysql/client-cert.pem
      client-key:
        src: ../../../files/percona-server/etc/mysql/client-key.pem
        dest: /etc/mysql/client-key.pem
      server-cert:
        src: ../../../files/percona-server/etc/mysql/server-cert.pem
        dest: /etc/mysql/server-cert.pem
      server-key:
        src: ../../../files/percona-server/etc/mysql/server-key.pem
        dest: /etc/mysql/server-key.pem
```

##### Configure replication

###### Master-slave

```yaml
- hosts: master
  roles:
    - percona-server
  vars:
    percona_server_users_present:
      - name: replicator
        password: 'replicator'
        privs:
          - '*.*:REPLICATION SLAVE'
        hosts:
          - '%'

    percona_server_configure_replication: true
    percona_server_server_id: 1
    percona_server_log_bin: mysql-bin
    percona_server_log_bin_index: mysql-bin.index
    percona_server_sync_binlog: 1

- hosts: slave
  roles:
    - percona-server
  vars:
      - name: replicator
        password: 'replicator'
        privs:
          - '*.*:REPLICATION SLAVE'
        hosts:
          - '%'

    percona_server_configure_replication: true
    percona_server_server_id: 2
    percona_server_relay_log: mysql-relay
    percona_server_relay_log_index: mysql-relay.index
    percona_server_sync_relay_log: 1

    percona_server_read_only: true
    percona_server_skip_slave_start: true
```

###### Master-master

```yaml
- hosts: master1
  roles:
    - percona-server
  vars:
    percona_server_users_present:
      - name: replicator
        password: 'replicator'
        privs:
          - '*.*:REPLICATION SLAVE'
        hosts:
          - '%'

    percona_server_configure_replication: true
    percona_server_server_id: 1
    percona_server_log_bin: mysql-bin
    percona_server_log_bin_index: mysql-bin.index
    percona_server_sync_binlog: 1
    percona_server_relay_log: mysql-relay
    percona_server_relay_log_index: mysql-relay.index
    percona_server_sync_relay_log: 1

    percona_server_skip_slave_start: true

- hosts: master2
  roles:
    - percona-server
  vars:
    percona_server_users_present:
      - name: replicator
        password: 'replicator'
        privs:
          - '*.*:REPLICATION SLAVE'
        hosts:
          - '%'

    percona_server_configure_replication: true
    percona_server_server_id: 2
    percona_server_log_bin: mysql-bin
    percona_server_log_bin_index: mysql-bin.index
    percona_server_sync_binlog: 1
    percona_server_relay_log: mysql-relay
    percona_server_relay_log_index: mysql-relay.index
    percona_server_sync_relay_log: 1

    percona_server_skip_slave_start: true
```

#### License

MIT

#### Author Information

Mischa ter Smitten (based on work of [overdrive3000](https://github.com/overdrive3000/ansible-percona), [geerlingguy](https://github.com/geerlingguy/ansible-role-mysql) and [silviud](https://gist.github.com/silviud/6382400))

#### Feedback, bug-reports, requests, ...

Are [welcome](https://github.com/Oefenweb/ansible-percona-server/issues)!
