<img src="http://www.elao.com/images/corpo/logo_red_small.png"/>

# Ansible Role: varnish

This role will assume the setup of [varnish](https://www.varnish-cache.org/)

It's part of the ELAO [Ansible stack](http://ansible.elao.com) but can be used as a stand alone component.

## Requirements

- Ansible 1.7.2+

## Dependencies

None

## Installation

Using ansible galaxy:

```bash
ansible-galaxy install elao.varnish
```
You can add this role as a dependency for other roles by adding the role to the meta/main.yml file of your own role:

```yaml
dependencies:
  - { role: elao.varnish }
```

## Role Handlers

    varnish restart  # Restart varnish server

<!-- -->

    varnishncsa restart  # Restart varnishncsa

## Role Variables

### Varnish daemon configuration

|Name|Default|Type|Description|
|----|----|-----------|-------|
|`elao_varnish_config_template`|config/default.j2|String (path)|Path to varnish config template. Stored in /etc/default/varnish|
|`elao_varnish_config.listen_address`|127.0.0.1|String (address)| Varnish listen for client requests on the specified addresses.|
|`elao_varnish_config.listen_port`|80|Integer (port)|Varnish listen port. |
|`elao_varnish_config.vcl_filepath`|/etc/varnish/default.vcl|String (path)| Main vcl file path. |
|`elao_varnish_config.admin_listen_address`|127.0.0.1|String| Offer a management interface on the specified address. |
|`elao_varnish_config.admin_listen_port`|6082|Integer (port)| Offer a management interface on the specified port. |
|`elao_varnish_config.secret_filepath`|/etc/varnish/secret|String (path)| Path to a file containing a secret used for authorizing access to the management interface. |
|`elao_varnish_config.ttl`|120|Integer| The TTL assigned to objects if neither the backend nor the VCL code assigns one. |
|`elao_varnish_config.storage`|malloc,256m|String| Varnish backend storage |
|`elao_varnish_config.instance`|-|String| Optional. Specify a name for this varnish instance. |

### Varnish vcls configuration

|Name|Default|Type|Description|
|----|----|-----------|-------|
|`elao_varnish_vcls`|Array|Dictionnary|A vcl list. Each vcl require a `name` and a `template` filepath. |

### Varnish ncsa (logging) configuration

|Name|Default|Type|Description|
|----|----|-----------|-------|
|`elao_varnish_config_ncsa_template`|config/varnishncsa.j2|String (path)|Path to varnishncsa config template. Stored in /etc/default/varnishncsa|
|`elao_varnish_config_ncsa.enabled`|false|Boolean| Enable logging for varnish |
|`elao_varnish_config_ncsa.format`|<code>%h&#124;%u&#124;%{%Y-%m-%d}t&#124;%{%H:%M:%S}t&#124;%{%z}t&#124;%m&#124;%{Host}i&#124;%U&#124;%q&#124;%s&#124;%b&#124;%{Referer}i&#124;%{User-agent}i&#124;%{Varnish:time_firstbyte}x&#124;%T&#124;%D&#124;%{Varnish:handling}x&#124;%{X-FE-Varnish-Obj-TTL}o&#124;%{X-FE-Varnish-Backend}o&#124;%{X-FE-Varnish-Obj-Stat}o</code>|String| [Log format](https://www.varnish-cache.org/docs/trunk/reference/varnishncsa.html#format)|

## Configuration examples

### Varnish with custom vcls

```
elao_varnish_vcls:
  - name: default.vcl
    template: "{{ playbook_dir ~ '/templates/varnish/default.vcl.j2' }}"
    backends:
      - name: default
        address: 127.0.0.1
        port: 80
  - name: vcl/errors/error404.vcl
    template: "{{ playbook_dir ~ '/templates/varnish/error404.vcl.j2' }}"
```

### Configure varnish daemon

```
elao_varnish_config:
  instance: varnish1
  listen_address: 0.0.0.0
  listen_port: 6789
```

### Configure varnish logging

```
elao_varnish_config_ncsa:
  enabled: false
  format: '%h|%l|%u|%t|"%r"|%s|%b|"%{Referer}i"|"%{User-agent}i"'
```

## Example playbook

    - hosts: servers
      roles:
        - { role: elao.varnish}

# Licence

MIT

# Author information

ELAO [**(http://www.elao.com/)**](http://www.elao.com)
