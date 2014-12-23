Fork of [million12/logstash-forwarder](https://registry.hub.docker.com/u/million12/logstash-forwarder/).

Removed automatic download of key/cert stuff from `etcd`.

Simply mount your key/crt files for lumberjack protocol to `/opt/logstash/ssl`.  Should be two files: `logstash-forwarder.key` and `logstash-forwarder.crt`.

Mount the host's `/var/log` to this container's `/data/log` in order for the default `forwarder.conf` to find your `system.log`, which is all the default behavior listens to.

You can mount your `forwarder.conf` to `/etc/forwarder/forwarder.conf` if you don't want to use the default specified in this repo.  If you do this, make sure the SSL and Logstash IP match are configured correctly for your environment, since without using the built-in `forwarder.conf`, you are responsible for making sure the paths and IP are correct (`LOGSTASH_IP` has no affect in this case).

Example using defalts:

    docker run \
      -v /var/log:/data/log \
      -v /my/ssl/directory:/opt/logstash/ssl \
      -e "LOGSTASH_IP=<your_logstash_ip>" \
      seanadkinson/logstash-forwarder

Example with your own configuration:

    docker run \
      -v /var/log:/data/log
      -v /my/ssl/directory:/opt/logstash/ssl
      -v /my/logstash/forwarder.conf:/etc/forwarder/forwarder.conf
      seanadkinson/logstash-forwarder
