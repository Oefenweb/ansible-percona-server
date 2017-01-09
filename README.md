## percona-server

[![Build Status](https://travis-ci.org/Oefenweb/ansible-percona-server.svg?branch=master)](https://travis-ci.org/Oefenweb/ansible-percona-server) [![Ansible Galaxy](http://img.shields.io/badge/ansible--galaxy-percona--server-blue.svg)](https://galaxy.ansible.com/tersmitten/percona-server)

Set up a [percona-server](https://www.percona.com/software/mysql-database/percona-server) server in Debian-like systems.

#### Requirements

* `python-mysqldb` (will be installed)

#### Variables

##### General

* `percona_server_version`: [default: `5.6`]: Version to install (e.g. `5.6`)
* `percona_server_root_password`: [default: `+eswuw9uthUteFreyAqu`]: Root password **Make sure to change!**

* `percona_server_install`: [`['xtrabackup']`]: Additional packages to install

* `percona_server_etc_my_cnf`: [default: `[]`]: Global configuration declarations
* `percona_server_etc_my_cnf_includedir`: [optional]: Used to include other option files from this directory (e.g. `/etc/mysql/conf.d/`)

* `percona_server_user_root_cnf_manage`: [default: `true`]: Whether or not to manage `~root/.my.cnf`
* `percona_server_user_root_cnf`: [default: `percona_server_user_root_cnf_preset`, see `defaults/main.yml`]: Root user configuration declarations

##### SSL

* `percona_server_ssl_map`: [default: `{}`]: SSL declarations
* `percona_server_ssl_map.key`: [required]: The identifier of the file (e.g. `ca-cert`)
* `percona_server_ssl_map.key.src`: [required]: The local path of the file to copy, can be absolute or relative (e.g. `../../../files/percona-server/etc/mysql/ca-cert.pem`)
* `percona_server_ssl_map.key.dest`: [required]: The remote path of the file to copy (e.g. `/etc/mysql/ca-cert.pem`)
* `percona_server_ssl_map.key.owner`: [optional, default `root`]: The name of the user that should own the file
* `percona_server_ssl_map.key.group`: [optional, default `mysql`]:The name of the group that should own the file
* `percona_server_ssl_map.key.mode`: [optional, default `0640`]: The mode of the file

##### Plugins

* `percona_server_plugins_present`: [default: `[]`]: Plugins to `INSTALL`
* `percona_server_plugins_present.{n}.name`: [required]: The name of the plugin (e.g. `QUERY_RESPONSE_TIME_AUDIT`)
* `percona_server_plugins_present.{n}.soname`: [required]: The base name of the shared library file that contains the code that implements the plugin (e.g. `query_response_time.so`)

* `percona_server_plugins_absent`: [default: `[]`]: Plugins to `UNINSTALL`
* `percona_server_plugins_absent.{n}.name`: [required]: The name of the plugin

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
* `percona-server-tools` ([see](https://github.com/Oefenweb/ansible-percona-server-tools))
* `percona-toolkit` ([see](https://github.com/Oefenweb/ansible-percona-toolkit))
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
    percona_server_etc_my_cnf:
      - section: client
        options:
          - name: ssl_cert
            value: "{{ percona_server_ssl_map['client-cert']['dest'] }}"
          - name: ssl_key
            value: "{{ percona_server_ssl_map['client-key']['dest'] }}"
      - section: mysqld
        options:
          - name: ssl_ca
            value: "{{ percona_server_ssl_map['ca-cert']['dest'] }}"
          - name: ssl_cert
            value: "{{ percona_server_ssl_map['server-cert']['dest'] }}"
          - name: ssl_key
            value: "{{ percona_server_ssl_map['server-key']['dest'] }}"
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

    percona_server_etc_my_cnf:
      - section: mysqld
        options:
          - name: server_id
            value: 1
          - name: log_bin
            value: mysql-bin
          - name: log_bin_index
            value: mysql-bin.index
          - name: sync_binlog
            value: 1
          - name: report_host
            value: "{{ inventory_hostname }}"

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

    percona_server_etc_my_cnf:
      - section: mysqld
        options:
          - name: server_id
            value: 2
          - name: relay_log
            value: mysql-relay
          - name: relay_log_index
            value: mysql-relay.index
          - name: sync_relay_log
            value: 1
          - name: report_host
            value: "{{ inventory_hostname }}"

          - name: read_only
            value: 1
          - name: skip_slave_start
            value: 1
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

    percona_server_etc_my_cnf:
      - section: mysqld
        options:
          - name: server_id
            value: 1
          - name: log_bin
            value: mysql-bin
          - name: log_bin_index
            value: mysql-bin.index
          - name: sync_binlog
            value: 1
          - name: relay_log
            value: mysql-relay
          - name: relay_log_index
            value: mysql-relay.index
          - name: sync_relay_log
            value: 1
          - name: report_host
            value: "{{ inventory_hostname }}"

          - name: skip_slave_start
            value: 1

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

    percona_server_etc_my_cnf:
      - section: mysqld
        options:
          - name: server_id
            value: 2
          - name: log_bin
            value: mysql-bin
          - name: log_bin_index
            value: mysql-bin.index
          - name: sync_binlog
            value: 1
          - name: relay_log
            value: mysql-relay
          - name: relay_log_index
            value: mysql-relay.index
          - name: sync_relay_log
            value: 1
          - name: report_host
            value: "{{ inventory_hostname }}"

          - name: skip_slave_start
            value: 1
```

#### License

MIT

#### Author Information

Mischa ter Smitten (based on work of [overdrive3000](https://github.com/overdrive3000/ansible-percona), [geerlingguy](https://github.com/geerlingguy/ansible-role-mysql) and [silviud](https://gist.github.com/silviud/6382400))

#### Feedback, bug-reports, requests, ...

Are [welcome](https://github.com/Oefenweb/ansible-percona-server/issues)!
