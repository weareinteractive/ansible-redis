# Ansible weareinteractive.redis role

[![Build Status](https://img.shields.io/travis/weareinteractive/ansible-redis.svg)](https://travis-ci.org/weareinteractive/ansible-redis)
[![Galaxy](http://img.shields.io/badge/galaxy-weareinteractive.redis-blue.svg)](https://galaxy.ansible.com/weareinteractive/redis)
[![GitHub Tags](https://img.shields.io/github/tag/weareinteractive/ansible-redis.svg)](https://github.com/weareinteractive/ansible-redis)
[![GitHub Stars](https://img.shields.io/github/stars/weareinteractive/ansible-redis.svg)](https://github.com/weareinteractive/ansible-redis)

> `weareinteractive.redis` is an [Ansible](http://www.ansible.com) role which:
>
> * installs redis
> * configures redis
> * configures service

**Note:**

> Since Ansible Galaxy supports [organization](https://www.ansible.com/blog/ansible-galaxy-2-release) now, this role has moved from `franklinkim.redis` to `weareinteractive.redis`!

## Installation

Using `ansible-galaxy`:

```shell
$ ansible-galaxy install weareinteractive.redis
```

Using `requirements.yml`:

```yaml
- src: weareinteractive.redis
```

Using `git`:

```shell
$ git clone https://github.com/weareinteractive/ansible-redis.git weareinteractive.redis
```

## Dependencies

* Ansible >= 2.0

## Variables

Here is a list of all the default variables for this role, which are also available in `defaults/main.yml`.

```yaml
---
# Accept connections on the specified port, default is 6379.
# If port 0 is specified Redis will not listen on a TCP socket.
redis_port: 6379
# By default Redis does not run as a daemon. Use 'yes' if you need it.
# Note that Redis will write a pid file in /var/run/redis.pid when daemonized.
redis_daemonize: yes
# By default Redis listens for connections from all the network interfaces
# available on the server. It is possible to listen to just one or multiple
# interfaces using the "bind" configuration directive, followed by one or
# more IP addresses.
redis_bind: []
# Specify the server verbosity level.
# This can be one of:
# debug (a lot of information, useful for development/testing)
# verbose (many rarely useful info, but not a mess like the debug level)
# notice (moderately verbose, what you want in production probably)
# warning (only very important / critical messages are logged)
redis_loglevel: notice
# Set the number of databases. The default database is DB 0, you can select
# a different one on a per-connection basis using SELECT <dbid> where
# dbid is a number between 0 and 'databases'-1
redis_databases: 16
# Save the DB on disk <seconds> <changes>
redis_save:
  - 900 1
  - 300 10
  - 60 10000
# apt repository
redis_repository: ppa:chris-lea/redis-server
# package name (version)
redis_package: redis-server
# start on boot
redis_service_enabled: yes
# current state: started, stopped
redis_service_state: started

```

## Handlers

These are the handlers that are defined in `handlers/main.yml`.

```yaml
---
- name: restart redis-server
  service:
    name: redis-server
    state: restarted
  when: redis_service_state != 'stopped'

```


## Usage

This is an example playbook:

```yaml
---

- hosts: all
  sudo: yes
  roles:
    - franklinkim.redis
  vars:
    redis_bind:
      - 127.0.0.1
    redis_databases: 2

```

## Testing

```shell
$ git clone https://github.com/weareinteractive/ansible-redis.git
$ cd ansible-redis
$ vagrant up
```

## Contributing
In lieu of a formal styleguide, take care to maintain the existing coding style. Add unit tests and examples for any new or changed functionality.

1. Fork it
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Add some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create new Pull Request

*Note: To update the `README.md` file please install and run `ansible-role`:*

```shell
$ gem install ansible-role
$ ansible-role docgen
```

## License
Copyright (c) We Are Interactive under the MIT license.
