Used to set up a virtual host with some options specific to Phusion Passenger, such as `PassengerRuby`.

Example usage:


    - { role: leihs_instance,
        server_name: "leihs.local",
        document_root: "/home/leihs/production/current/public",
        passenger_ruby: "/home/leihs/.rvm/wrappers/ruby-2.1.5/ruby",
        max_requests: 10000,
        ssl: true,
        sslredirect: true,
        certificate_file: "leihs.local.crt",
        certificate_chain_file: "leihs.local.bundle.crt",
        certificate_key_file: "leihs.local.key" }
        }

See defaults/main.yml for the default settings of those options.


## Options

*sslredirect*: Boolean. Controls whether an automatic redirect from the hostname mentioned under `server_name`:80 to `server_name`:443 should be installed.
