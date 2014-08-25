## percona-server [![Build Status](https://travis-ci.org/Oefenweb/ansible-percona-server.svg?branch=master)](https://travis-ci.org/Oefenweb/ansible-percona-server)

Set up a percona-server server in Debian-like systems.

#### Requirements

None

#### Variables

None

## Dependencies

None

#### Example

```yaml
---
- hosts: all
  roles:
  - percona-server
```

#### TODO

* Install Percona Toolkit UDFs
* Create ~/.my.cnf
* Replication

#### License

MIT

#### Author Information

Mischa ter Smitten (based on work of overdrive3000 and silviud)

#### Feedback, bug-reports, requests, ...

Are [welcome](https://github.com/Oefenweb/ansible-percona-server/issues)!
