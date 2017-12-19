# Ansible Role: Splunk Universal Forwarder

Installs and configures the Splunk Universal Forwarder on all unix systems.


## Requirements

None.

## Role Variables

Below are available variables for configuring the forwarder, along with default values (from `defaults/main.yml`):

```
# Installation options:
# Role installs binary to /opt/splunkforwarder - currently, non-configurable
# ---
splunk_uf_system_user: splunk  # The splunk forwarder will run as this user
splunk_uf_start_on_boot: yes
splunk_uf_install_url: https://download.splunk.com/products/universalforwarder/releases/6.6.4/linux/splunkforwarder-6.6.4-00895e76d346-Linux-x86_64.tgz
# Installations are done via the "universal" binary method instead of relying on the RPM or DEB package - it's easier to
# install from the binary and it should work beyond RedHat/CentOS and Debian systems

# Configuration for inputs.conf
# ---
splunk_uf_inputs_conf_template: inputs.conf.j2
# Input attributes below are all optional
splunk_uf_inputs_default_host: ""
splunk_uf_inputs_default_index: ""
splunk_uf_inputs_default_extra_attributes: ""
# Example of extra attributes within the default stanza:
#   splunk_uf_inputs_default_extra_options: |
#     whitelist = \.log$
#     ignoreOlderThan = 7
splunk_uf_inputs_monitors: []
# Example monitor:
#   splunk_uf_inputs_monitors:
#     - path: /var/log # leading // is included by template
#       # These properties are only added if defined
#       whitelist: \.log$
#       blacklist: \.gz$
#       ignoreOlderThan: 7
#       recursive: false

splunk_uf_inputs_additional_forwarders: ""
# Forwarders other than monitors, like:
#   splunk_uf_inputs_additional_forwarders: |
#     [tcp://:9995]
#     connection_host = dns
#     sourcetype = log4j
#     source = tcp:9995


# Configuration for outputs.conf
# ---
splunk_uf_outputs_conf_template: outputs.conf.j2
# Output attributes below are all optional
splunk_uf_outputs_default_group: ""
splunk_uf_outputs_default_extra_attributes: ""
# Example of extra attributes within default/tcp stanza:
#   splunk_uf_outputs_default_extra_options: |
#     compressed = true
#     blockOnCloning = false
#
splunk_uf_outputs_target_groups: []
# Example target group definition
#   splunk_uf_outputs_target_groups:
#     - name: group1
#       server: localhost:9997
#     - name: group2
#       server: 10.0.1.1:9996, 10.0.1.2:9995
#       # Only added if defined
#       compressed = true
```

## Overriding configuration templates

You can override the forwarder inputs and outputs templates if the exposed variables don't cover an option you want to define.  You can either copy and modify the provided template, or extend it with [Jinja2 template inheritance](http://jinja.pocoo.org/docs/2.9/templates/#template-inheritance) and override the specific template block you need to change.  To override, you'll
need to point one of the conf_template vars to your local template.

## Skipping a configuration

You may not want one of the local templates to be generated if, for example, you are pulling config from a deployment server
via the CLI.   To prevent the installation of a template configuration you can simply blank out the desired conf_template
location in your var declaration.  For example:
```
vars:
    splunk_uf_inputs_conf_template: ""
```

## Dependencies

None.

## Example Playbook

    - hosts: server
      roles:
         - { role: huit-at.splunk-uf }

## License

MIT

## Author Information

This role was created by Jaime Bermudez in 2017 as a member of HUIT's Academic Technology group.
