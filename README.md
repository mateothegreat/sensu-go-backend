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
        sensu_backend:
          delete_if_exists: "true"
          namespace: "default"
          state_dir: "/var/lib/sensu/sensu-backend"
          auth:
            username: "admin"
            password: "supersecret"
          api:
            url: "http://localhost:8080"
            port: "8080"
          agent:
            port: "8081"
          dashboard:
            port: "8082"
          default:
            interval: "15"
            timeout: "10"
            handlers:
              - "slack"
          assets:
        
            - name: "sensu/sensu-ruby-runtime"
              rename: "sensu-ruby-runtime"
              namespaces: [ "apps", "cam", "mlfabric", "pulse-corona", "relationalgraph", "sandbox" ]
            - name: "sensu-plugins/sensu-plugins-load-checks"
              rename: "load"
              namespaces: [ "apps", "cam", "mlfabric", "pulse-corona", "relationalgraph", "sandbox" ]
            - name: "sensu-plugins/sensu-plugins-disk-checks"
              rename: "disk"
              namespaces: [ "apps", "cam", "mlfabric", "pulse-corona", "relationalgraph", "sandbox" ]
            - name: "sensu-plugins/sensu-plugins-io-checks"
              rename: "io"
              namespaces: [ "apps", "cam", "mlfabric", "pulse-corona", "relationalgraph", "sandbox" ]
            - name: "sensu-plugins/sensu-plugins-memory-checks"
              rename: "memory"
              namespaces: [ "apps", "cam", "mlfabric", "pulse-corona", "relationalgraph", "sandbox" ]
            - name: "sensu-plugins/sensu-plugins-network-checks"
              rename: "network"
              namespaces: [ "apps", "cam", "mlfabric", "pulse-corona", "relationalgraph", "sandbox" ]
            - name: "sensu-plugins/sensu-plugins-process-checks"
              rename: "processes"
              namespaces: [ "apps", "cam", "mlfabric", "pulse-corona", "relationalgraph", "sandbox" ]
            - name: "sensu-plugins/sensu-plugins-http"
              rename: "http"
              namespaces: [ "apps", "cam", "mlfabric", "pulse-corona", "relationalgraph", "sandbox" ]
            - name: "sensu-plugins/sensu-plugins-redis"
              rename: "redis"
              namespaces: [ "apps", "cam", "mlfabric", "pulse-corona", "relationalgraph", "sandbox" ]
            - name: "sensu-plugins/sensu-plugins-elasticsearch"
              rename: "elasticsearch"
              namespaces: [ "apps", "cam", "mlfabric", "pulse-corona", "relationalgraph", "sandbox" ]
            - name: "sensu-plugins/sensu-plugins-influxdb"
              rename: "influxdb"
              namespaces: [ "apps", "cam", "mlfabric", "pulse-corona", "relationalgraph", "sandbox" ]
            - name: "betorvs/sensu-opsgenie-handler"
              rename: "opsgenie"
              namespaces: [ "apps", "cam", "mlfabric", "pulse-corona", "relationalgraph", "sandbox" ]
            - name: "sensu-plugins/sensu-plugins-aws"
              rename: "aws"
              namespaces: [ "apps", "cam", "mlfabric", "pulse-corona", "relationalgraph", "sandbox" ]
        
          checks:
        
            - name: "disk"
              namespaces: [ "default", "cam", "pulse-corona", "relationalgraph", "mlfabric" ]
              command: "check-disk-usage.rb -w 70 -c 80"
              assets: [ "disk" ]
              subscriptions: [ "all" ]
        
            - name: "memory"
              namespaces: [ "default", "cam", "pulse-corona", "relationalgraph", "mlfabric" ]
              command: "check-memory.rb -w 75 -c 90"
              assets: [ "memory" ]
              subscriptions: [ "all" ]
        
            - name: "node_exporter"
              namespaces: [ "default", "cam", "pulse-corona", "relationalgraph", "mlfabric" ]
              command: "check-process.rb -W 1 -c 2 -p 'prometheus-exporter-node'"
              assets: [ "processes" ]
              subscriptions: [ "all" ]
        
            - name: "filebeat"
              namespaces: [ "default", "cam", "pulse-corona", "relationalgraph", "mlfabric" ]
              command: "check-process.rb -W 1 -c 2 -p 'filebeat'"
              assets: [ "processes" ]
              subscriptions: [ "all" ]
        
            - name: "elasticsearch-node-status"
              namespaces: [ "mlfabric" ]
              command: "check-es-node-status.rb --host $(ip -o route get to 1.1.1.1 | sed -n 's/.*src \\([0-9.]\\+\\).*/\\1/p')"
              assets: [ "elasticsearch" ]
              subscriptions: [ "elasticsearch" ]
        
            - name: "elasticsearch-rest-api"
              namespaces: [ "mlfabric" ]
              command: "check-http.rb -u http://$(ip -o route get to 1.1.1.1 | sed -n 's/.*src \\([0-9.]\\+\\).*/\\1/p'):9200"
              assets: [ "http" ]
              subscriptions: [ "elasticsearch" ]
        
            - name: "elasticsearch-kibana-ui"
              namespaces: [ "mlfabric" ]
              command: "check-http.rb -u http://$(ip -o route get to 1.1.1.1 | sed -n 's/.*src \\([0-9.]\\+\\).*/\\1/p'):5601/app/home"
              assets: [ "http" ]
              subscriptions: [ "kibana" ]
        
            - name: "app-mlfabric-ui"
              namespaces: [ "apps" ]
              command: "/usr/lib64/nagios/plugins/check_http -H mlfabric.moodysanalytics.com"
              assets: [ "http" ]
              subscriptions: [ "apps" ]
        
            - name: "app-mlfabric-api"
              namespaces: [ "apps" ]
              command: "/usr/lib64/nagios/plugins/check_http -H api.mlfabric.moodysanalytics.com -e 403"
              assets: [ "http" ]
              subscriptions: [ "apps" ]
        
            - name: "app-pulse-coronra-ui"
              namespaces: [ "apps" ]
              command: "/usr/lib64/nagios/plugins/check_http -H pulse.moodysanalytics.com"
              assets: [ "http" ]
              subscriptions: [ "apps" ]
        
            - name: "app-pulse-corona-api"
              namespaces: [ "apps" ]
              command: "/usr/lib64/nagios/plugins/check_http --ssl -H lsclydaje0.execute-api.us-east-1.amazonaws.com -u /prod/top-issuers -e 403"
              assets: [ "http" ]
              subscriptions: [ "apps" ]
        
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
