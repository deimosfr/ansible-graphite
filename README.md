Ansible Graphite Role
=====

This role installs and configures Graphite.

Requirements
------------

This role requires Ansible 1.6 or higher and platform requirements are listed
in the metadata file.

It's also able to configure mysql/mariadb if it's already installed. No meta data have been added for this part because you may depend on another role. However deimosfr.mariadb is strongly recommanded.

Role Variables
--------------

The variables that can be passed to this role:

```
# Account
graphite_admin_username: admin
graphite_admin_email: admin@admin.com

# MySQL/MariaDB
graphite_manage_db: true
graphite_db_backend: mysql
graphite_db_dbname: graphite
graphite_db_user: graphite
graphite_db_password: graphite
graphite_db_dbname: graphite
graphite_db_host: localhost
graphite_db_port: 3306

# Webapp secret key
graphite_webapp_secret_key: "SECRET_KEY"

# Webapp
graphite_webapp:
  - name: 'general_configuration'
    config:
      - SECRET_KEY = "{{graphite_webapp_secret_key}}"
      #- ALLOWED_HOSTS = [ '*' ]
      #- DOCUMENTATION_URL = "http://graphite.readthedocs.org/"
      - TIME_ZONE = 'Europe/Paris'
      - LOG_RENDERING_PERFORMANCE = True
      - LOG_CACHE_PERFORMANCE = True
      - LOG_METRIC_ACCESS = True
      #-DEBUG = True
      #-FLUSHRRDCACHED = 'unix:/var/run/rrdcached.sock'
      #-MEMCACHE_HOSTS = ['10.10.10.10:11211', '10.10.10.11:11211', '10.10.10.12:11211']
      #-DEFAULT_CACHE_DURATION = 60
  - name: 'filesystem_path'
    config:
      - GRAPHITE_ROOT = '/usr/share/graphite-web'
      - CONF_DIR = '/etc/graphite'
      - STORAGE_DIR = '/var/lib/graphite/whisper'
      - CONTENT_DIR = '/usr/share/graphite-web/static'
      #-DASHBOARD_CONF = '/opt/graphite/conf/dashboard.conf'
      #-GRAPHTEMPLATES_CONF = '/opt/graphite/conf/graphTemplates.conf'
      - WHISPER_DIR = '/var/lib/graphite/whisper'
      - LOG_DIR = '/var/log/graphite'
      - INDEX_FILE = '/var/lib/graphite/search_index'
  #- name: 'email_configuration'
    #config:
      #-EMAIL_BACKEND = 'django.core.mail.backends.smtp.EmailBackend'
      #-EMAIL_HOST = 'localhost'
      #-EMAIL_PORT = 25
      #-EMAIL_HOST_USER = ''
      #-EMAIL_HOST_PASSWORD = ''
      #-EMAIL_USE_TLS = False
      #-EMAIL_BACKEND = 'django.core.mail.backends.dummy.EmailBackend'
  #- name: 'authentication_configuration'
    #config:
      #-USE_LDAP_AUTH = True
      #-LDAP_SERVER = "ldap.mycompany.com"
      #-LDAP_PORT = 389
      #-LDAP_URI = "ldaps://ldap.mycompany.com:636"
      #-LDAP_SEARCH_BASE = "OU=users,DC=mycompany,DC=com"
      #-LDAP_BASE_USER = "CN=some_readonly_account,DC=mycompany,DC=com"
      #-LDAP_BASE_PASS = "readonly_account_password"
      #-LDAP_USER_QUERY = "(username=%s)"  #For Active Directory use "(sAMAccountName=%s)"
      #-USE_REMOTE_USER_AUTHENTICATION = True
      #-LOGIN_URL = '/account/login'
  #- name: 'cluster_configuration'
    #config:
      #-CLUSTER_SERVERS = ["10.0.2.2:80", "10.0.2.3:80"]
      #-REMOTE_STORE_FETCH_TIMEOUT = 6   # Timeout to fetch series data
      #-REMOTE_STORE_FIND_TIMEOUT = 2.5  # Timeout for metric find requests
      #-REMOTE_STORE_RETRY_DELAY = 60    # Time before retrying a failed remote webapp
      #-REMOTE_FIND_CACHE_DURATION = 300 # Time to cache remote metric find results
      #-REMOTE_RENDERING = True
      #-RENDERING_HOSTS = []
      #-REMOTE_RENDER_CONNECT_TIMEOUT = 1.0
      #-CARBONLINK_HOSTS = ["127.0.0.1:7002:a", "127.0.0.1:7102:b", "127.0.0.1:7202:c"]
      #-CARBONLINK_TIMEOUT = 1.0
  #- name: 'django_settings'
    #config:
      #-from graphite.app_settings import *

# Carbon
graphite_carbon:
  - name: 'cache'
    config:
      - STORAGE_DIR    = /var/lib/graphite/
      - CONF_DIR       = /etc/carbon/
      - LOG_DIR        = /var/log/carbon/
      - PID_DIR        = /var/run/
      - LOCAL_DATA_DIR = /var/lib/graphite/whisper/
      - ENABLE_LOGROTATION = False
      - USER = _graphite
      - MAX_CACHE_SIZE = inf
      - MAX_UPDATES_PER_SECOND = 500
      #- MAX_UPDATES_PER_SECOND_ON_SHUTDOWN = 1000
      - MAX_CREATES_PER_MINUTE = 50
  - name: 'relay'
    config:
      - LINE_RECEIVER_INTERFACE = 0.0.0.0
      - LINE_RECEIVER_PORT = 2003
      - ENABLE_UDP_LISTENER = False
      - UDP_RECEIVER_INTERFACE = 0.0.0.0
      - UDP_RECEIVER_PORT = 2003
  - name: 'aggregator'
    config:
      - PICKLE_RECEIVER_INTERFACE = 0.0.0.0
      - PICKLE_RECEIVER_PORT = 2004
      - LOG_LISTENER_CONNECTIONS = True
      - USE_INSECURE_UNPICKLER = False
      - CACHE_QUERY_INTERFACE = 0.0.0.0
      - CACHE_QUERY_PORT = 7002
      - USE_FLOW_CONTROL = True
      - LOG_UPDATES = False
      - LOG_CACHE_HITS = False
      - LOG_CACHE_QUEUE_SORTS = True
      - CACHE_WRITE_STRATEGY = sorted
      - WHISPER_AUTOFLUSH = False
      #- WHISPER_SPARSE_CREATE = False
      - WHISPER_FALLOCATE_CREATE = True
      #- WHISPER_LOCK_WRITES = False
      #- USE_WHITELIST = False
      #- CARBON_METRIC_PREFIX = carbon
      #- CARBON_METRIC_INTERVAL = 60
      #- ENABLE_AMQP = False
      #- AMQP_VERBOSE = False
      #- AMQP_HOST = localhost
      #- AMQP_PORT = 5672
      #- AMQP_VHOST = /
      #- AMQP_USER = guest
      #- AMQP_PASSWORD = guest
      #- AMQP_EXCHANGE = graphite
      #- AMQP_METRIC_NAME_IN_BODY = False
      #- ENABLE_MANHOLE = False
      #- MANHOLE_INTERFACE = 127.0.0.1
      #- MANHOLE_PORT = 7222
      #- MANHOLE_USER = admin
      #- MANHOLE_PUBLIC_KEY = ssh-rsa AAAAB3NzaC1yc2EAAAABiwAaAIEAoxN0sv/e4eZCPpi3N3KYvyzRaBaMeS2RsOQ/cDuKv11dlNzVeiyc3RFmCv5Rjwn/lQ79y0zyHxw67qLyhQ/kDzINc4cY41ivuQXm2tPmgvexdrBv5nsfEpjs3gLZfJnyvlcVyWK/lId8WUvEWSWHTzsbtmXAF2raJMdgLTbQ8wE=
      #- BIND_PATTERNS = sales.#, servers.linux.#, #.utilization
      #- BIND_PATTERNS = #
      #-LINE_RECEIVER_PORT = 2103
      #-PICKLE_RECEIVER_PORT = 2104
      #-CACHE_QUERY_PORT = 7102
      #- You can then specify the --instance=b option to manage this instance
      - LINE_RECEIVER_INTERFACE = 0.0.0.0
      - LINE_RECEIVER_PORT = 2013
      - PICKLE_RECEIVER_INTERFACE = 0.0.0.0
      - PICKLE_RECEIVER_PORT = 2014
      - LOG_LISTENER_CONNECTIONS = True
      #-RELAY_METHOD = rules
      #-RELAY_METHOD = consistent-hashing
      #-RELAY_METHOD = aggregated-consistent-hashing
      - RELAY_METHOD = rules
      - REPLICATION_FACTOR = 1
      #- DESTINATIONS = 127.0.0.1:2004:a, 127.0.0.1:2104:b
      #- If using RELAY_METHOD = rules, all destinations used in relay-rules.conf
      - DESTINATIONS = 127.0.0.1:2004
      - MAX_DATAPOINTS_PER_MESSAGE = 500
      - MAX_QUEUE_SIZE = 10000
      - USE_FLOW_CONTROL = True
      #- USE_WHITELIST = False
      #- CARBON_METRIC_PREFIX = carbon
      #- CARBON_METRIC_INTERVAL = 60
      - LINE_RECEIVER_INTERFACE = 0.0.0.0
      - LINE_RECEIVER_PORT = 2023
      - PICKLE_RECEIVER_INTERFACE = 0.0.0.0
      - PICKLE_RECEIVER_PORT = 2024
      - LOG_LISTENER_CONNECTIONS = True
      - FORWARD_ALL = True
      #- DESTINATIONS = 127.0.0.1:2004:a, 127.0.0.1:2104:b
      - DESTINATIONS = 127.0.0.1:2004
      - REPLICATION_FACTOR = 1
      - MAX_QUEUE_SIZE = 10000
      - USE_FLOW_CONTROL = True
      - MAX_DATAPOINTS_PER_MESSAGE = 500
      - MAX_AGGREGATION_INTERVALS = 5
      #- By default (WRITE_BACK_FREQUENCY = 0), carbon-aggregator will write back
      #- Set this (WRITE_BACK_FREQUENCY = N) to write back all aggregated data points
      #- WRITE_BACK_FREQUENCY = 0
      #- USE_WHITELIST = False
      #- CARBON_METRIC_PREFIX = carbon
      #- CARBON_METRIC_INTERVAL = 60

# Storage schema
graphite_storage_schema:
  - name: 'carbon'
    config:
      - pattern = ^carbon\.
      - retentions = 60:90d
  - name: 'default_1min_for_1day'
    config:
      - pattern = .*
      - retentions = 60s:1d

# Storage schema
graphite_storage_aggregation:
  - name: 'min'
    config:
      - pattern = \.min$
      - xFilesFactor = 0.1
      - aggregationMethod = min
  - name: 'max'
    config:
      - pattern = \.max$
      - xFilesFactor = 0.1
      - aggregationMethod = max
  - name: 'sum'
    config:
      - pattern = \.count$
      - xFilesFactor = 0
      - aggregationMethod = sum
  - name: 'default_average'
    config:
      - pattern = .*
      - xFilesFactor = 0.5
      - aggregationMethod = average
```


Examples
========

```
# Roles
- name: graphite
  hosts: graphite
  user: root
  roles:
    - deimosfr.mariadb
    - deimosfr.graphite
    - role: jdauphant.nginx
      nginx_sites:
        graphite.fqdn:
          - listen 80
          - server_name graphite.fqdn
          - root /usr/share/graphite-web
          - access_log /var/log/nginx/graphite_access.log nm-combined
          - error_log /var/log/nginx/graphite_error.log
          - error_page   500 502 503 504  /50x.html
          - location = /50x.html {
              root /usr/share/nginx/html;
            }
          - location /content {
              alias /usr/share/graphite-web/static;
            }
          - location / {
              include uwsgi_params;
              uwsgi_pass unix:/run/uwsgi/app/graphite/socket;
            }
  vars:
    - graphite_webapp_secret_key: 'A_BETTER_PASSWORD'
```

Dependencies
------------

MySQL/MariaDB if you want to use it as your backend.

License
-------

GPL2

Author Information
------------------

Pierre Mavro / deimosfr

