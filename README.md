Docker image for ISC DHCP server
================================

[![](https://images.microbadger.com/badges/image/networkboot/dhcpd.svg)](https://microbadger.com/images/networkboot/dhcpd "See more networkboot/dhcpd image details")

This Docker image is suitable for running a DHCP server for your docker host
network.  It uses ISC DHCP server which is bundled with the latest Ubuntu
LTS distribution.

How to build
============

 1. Install Docker with the instructions on <https://www.docker.com>.
 2. Run `./build` to create the local docker image `networkboot/dhcpd`.

How to use
==========

The most common use-case is to provide DHCP service to the host network of
the machine running Docker.  For that you need to create a configuration for
the DHCP server, start the container with the `--net=host` docker run
option and specify the network interface you want to provide DHCP service
on.

 1. Create `data` folder.
 2. Create `data/dhcpd.conf` with a subnet clause for the specified
    network interface.  If you need assistance, you can run
    `docker run -it --rm networkboot/dhcpd man dhcpd.conf` for a description
    of the configuration file syntax.
 3. Run `docker -it --rm --net=host -v "$(pwd)/data":/data networkboot/dhcpd eth0`.
    `dhcpd` will automatically start and display its logs on the console.
    You can press Ctrl-C to terminate the server.

A simple `run` script is also included which makes it quick to iterate on a
configuration until you're satisfied.

Notes
=====

The entrypoint script in the docker image takes care of running the DHCP
server as the same user that owns the `data` folder.  This ensures that the
permissions on the files inside the `data` folder is kept consistent.  If
the `data` folder is owned by root, dhcpd is run as the normal dhcpd user.

If you forget to run the docker container with the `--net=host` option a
warning will be emitted informing you that you've probably forgotten it.

If a `/data` volume is not provided with a `dhcpd.conf` inside it, the
container will exit early with an error message.

Acknowledgements
================

This image uses the following software components:

 * Ubuntu Linux distribution from <https://www.ubuntu.com>.
 * ISC DHCP server from <https://www.isc.org/downloads/dhcp/>.
 * Docker-optimized my_init from <https://github.com/phusion/baseimage-docker>.

Copyright & License
===================

This project is copyright 2015 Robin Smidsrød <robin@smidsrod.no>.

It is licensed under the Apache 2.0 license.

See the file LICENSE for full legal details.
