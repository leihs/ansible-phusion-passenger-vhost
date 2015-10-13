Used to set up a virtual host with some options specific to Phusion Passenger, such as `PassengerRuby`.

This supports Debian 8.1 (jessie).

Example usage:


    - { role: leihs_instance,
        server_name: "leihs.local",
        document_root: "/home/leihs/production/current/public",
        passenger_ruby: "/home/leihs/.rvm/wrappers/ruby-2.1.5/ruby",
        max_requests: 10000,
        ssl: true,
        sslredirect: true,
        local_certificate_file: "/mnt/secret/certs/leihs.local.crt",
        local_certificate_chain_file: "/mnt/secret/certs/leihs.local.bundle.crt",
        local_certificate_key_file: "/mnt/secret/keys/leihs.local.key" }
        }

See defaults/main.yml for the default settings of those options.


## Options

**sslredirect**: Boolean. Controls whether an automatic redirect from the hostname mentioned under `server_name`:80 to `server_name`:443 should be installed.

**ssl**: Boolean. Whether or not SSL should be enabled and a virtual host should be created on port 443 and with SSL support. Requires at least certificate_file, certificate_key_file or local_certificate_file, local_certificate_key_file.

## Certificate files and storage

Do not commit certificates to the repository, instead store them somewhere securely and outside the playbook directory. There are two ways to achieve this:

 1. Store your certificates and key files on the leihs server and manage them independently from this playbook, giving their absolute paths on the remote server to the `certificate_*` options.
 2. Store them somewhere on your local machine or a securely shared file server, then give the absolute local path to the `local_certificate_*` options.


### If certificates are already stored on the leihs server

`certificate_file`: Absolute path **on the remote server** to an SSL certificate in PEM format.

`certificate_chain_file`: Absolute path **on the remote server** to an SSL certificate chain file in PEM format.

`local_certificate_key_file`: Absolute path to the SSL private key associated with this certificate, **on the remote server**.


### If certificates are to be copied over from a secure location

`local_certificate_file`: Absolute path **on your local machine** to your SSL certificate in PEM format. It will be copied to `/etc/ssl` on the destination host under the basename of the path (e.g. leihs.local.crt for the example above).

`local_certificate_chain_file`: Absolute path **on your local machine** to your SSL certificate chain file in PEM format.  It will be copied to `/etc/ssl` on the destination host under the basename of the path (e.g. leihs.local.bundle.crt for the example above).

`local_certificate_key_file`: Absolute path to the SSL private key associated with this certificate, **on your local machine**. It will be copied to `/etc/ssl/private` on the destination host under the basename of the path (e.g. leihs.local.bundle.crt for the example above). Permissions will be 0640 (`-rw-r-----`) owned by root.ssl-certs.


## Adding to your own Ansible dir

If you stick reasonably closely to Ansible's recommended directory layout and if you keep your Ansible stuff in a git repo, you can add this role as a submodule:

    git submodule add https://github.com/psy-q/ansible-phusion-passenger-vhost.git roles/ruby/passenger_vhost
    git submodule init
    git submodule update

Of course you use a target path that's cooler than mine.
