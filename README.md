## percona-server [![Build Status](https://travis-ci.org/Oefenweb/ansible-percona-server.svg?branch=master)](https://travis-ci.org/Oefenweb/ansible-percona-server)

Set up a percona-server server in Debian-like systems.

#### Requirements

* `python-mysqldb`

#### Variables

* `percona_server_ssl_map` [default: `{}`]: SSL declarations
* `percona_server_ssl_map.key`: The identifier of the file (e.g. `ca-cert`)
* `percona_server_ssl_map.key.src`: The local path of the file to copy, can be absolute or relative (e.g. `../../../files/percona-server/etc/mysql/ca-cert.pem`)
* `percona_server_ssl_map.key.dest`: The remote path of the file to copy (e.g. `/etc/mysql/ca-cert.pem`)
* `percona_server_ssl_map.key.owner`: The name of the user that should own the file (optional, default `root`)
* `percona_server_ssl_map.key.group`: The name of the group that should own the file (optional, default `mysql`)
* `percona_server_ssl_map.key.mode`: The mode of the file, such as 0644 (optional, default `0640`)

## Dependencies

None

#### Example

```yaml
---
- hosts: all
  roles:
  - percona-server
```

##### Configure SSL

```yaml
percona_server_ssl_map:
  ca-cert:
    src: ../../../files/percona-server/etc/mysql/ca-cert.pem
    dest: /etc/mysql/ca-cert.pem
  client-cert:
    src: ../../../files/percona-server/etc/mysql/ca-key.pem
    dest: /etc/mysql/ca-key.pem
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

#### TODO

* Replication
* Mention variables in README

#### License

MIT

#### Author Information

Mischa ter Smitten (based on work of overdrive3000 and silviud)

#### Feedback, bug-reports, requests, ...

Are [welcome](https://github.com/Oefenweb/ansible-percona-server/issues)!
