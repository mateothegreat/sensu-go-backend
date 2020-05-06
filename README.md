Role Name
=========

A brief description of the role goes here.

Requirements
------------

Any pre-requisites that may not be covered by Ansible itself or the role should be mentioned here. For instance, if the role uses the EC2 module, it may be a good idea to mention in this section that the boto package is required.

Role Variables
--------------

A description of the settable variables for this role should go here, including any variables that are in defaults/main.yml, vars/main.yml, and any variables that can/should be set via parameters to the role. Any variables that are read from other roles and/or the global scope (ie. hostvars, group vars, etc.) should be mentioned here as well.

Dependencies
------------

A list of other roles hosted on Galaxy should go here, plus any details in regards to parameters that may need to be set for other roles, or variables that are used from other roles.

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:
```yaml

- hosts: monitoring
  roles:
    - role: "mateothegreat.sensu_go_backend"
      vars:
        sensu:
          default:
            interval: "15"
            timeout: "10"
            handlers:
              - "slack"
          auth:
            admin:
              username: "admin"
              password: "supersecret"
          ports:
            api: "8080"
            agents: "8081"
            dashboard: "8082"
          assets:
            - name: "sensu/sensu-ruby-runtime"
              rename: "sensu-ruby-runtime"
            - name: "sensu-plugins/sensu-plugins-load-checks"
              rename: "load"
            - name: "sensu-plugins/sensu-plugins-disk-checks"
              rename: "disk"
            - name: "sensu-plugins/sensu-plugins-io-checks"
              rename: "io"
            - name: "sensu-plugins/sensu-plugins-memory-checks"
              rename: "memory"
            - name: "sensu-plugins/sensu-plugins-process-checks"
              rename: "processes"
            - name: "sensu-plugins/sensu-plugins-http"
              rename: "http"
            - name: "sensu-plugins/sensu-plugins-redis"
              rename: "redis"
            - name: "sensu-plugins/sensu-plugins-elasticsearch"
              rename: "elasticsearch"
            - name: "sensu-plugins/sensu-plugins-influxdb"
              rename: "influxdb"
            - name: "betorvs/sensu-opsgenie-handler"
              rename: "opsgenie"
            - name: "sensu/sensu-slack-handler"
              rename: "slack"
            - name: "sensu-plugins/sensu-plugins-aws"
              rename: "aws"
          checks:
            - name: "disk"
              command: "check-disk-usage.rb -w 70 -c 80"
              assets: [ "disk" ]
              subscriptions: [ "all" ]
            - name: "memory"
              command: "check-memory.rb -w 75 -c 90"
              assets: [ "memory" ]
              subscriptions: [ "all" ]
            - name: "node_exporter"
              command: "check-process.rb -W 1 -c 2 -p 'node_exporter'"
              assets: [ "processes" ]
              subscriptions: [ "all" ]
            - name: "filebeat"
              command: "check-process.rb -W 1 -c 2 -p 'filebeat'"
              assets: [ "processes" ]
              subscriptions: [ "all" ]
          handlers:
            - name: "opsgenie"
              env_vars:
                - "OPSGENIE_AUTHTOKEN=<your api token>"
                - "OPSGENIE_TEAM=<your team>"
              filters:
                - "is_incident"

```
License
-------

MIT

Author Information
------------------

An optional section for the role authors to include contact information, or a website (HTML is not allowed).
