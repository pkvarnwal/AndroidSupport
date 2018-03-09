
---

layout: post
title: "Mitmproxy"
categories:
- blog:
tags:
- Android

author: Prinsu-kumar
---


 ### Introduction

`Mitmproxy` is an interactive man-in-the-middle proxy for HTTP and HTTPS with a console
interface.

`Mitmdump` is the command-line version of mitmproxy. Think `tcpdump` for HTTP.

`Mitmweb` is a web-based interface for mitmproxy.

### Mitmproxy

Mitmproxy is a console tool that allows interactive examination and modification
of HTTP traffic. t differs from mitmdump in that all flows are kept in memory,
which means that it’s intended for taking and manipulating small-ish samples.

### How it works

`Mitmproxy` is an open source proxy application that allows intercepting HTTP and HTTPS connecitons
between any HTTP(S) client (such as a mobile or desktop browser) and a web server using a typical
`man-in-the-middle attack` (MITM). Similar to other proxies (such as `Squid`), it accepts connections
from clients and forward them to the destination server. However, while other proxies typically
focus on content filtering or speed optimization through caching, the goal `Mitmproxy`is to let
an attacker monitor, capture and alter these connections in real-time.

* ### 1.1 Attacking HTTP Connections

For unencrypted HTTP connections, this is quite simple: mitmproxy accepts a connection from the HTTP
client, use a mobile browser, displays the request(and its request parameters) to the attacker
on the screen, and forwards the request to the destination web server as soon as the attacker
confirms, maybe after `adjusting` the request payload a bit.
Mitmproxy simply act as a middle man: To the client, it looks like as if the Mitmproxy server was
simply relaying its connection (like your router or your ISP’s servers do).

* ### 1.2 Attacking HTTPS Connections

While attacking unencrypted HTTP traffic can be done without having to deal with X.509 certificates
and certificate authorities(CA), SSL-encrypted HTTPS connections encrypt every request and response
between client and server end-to-end. And because the transferred data is encrypted with a shared
secret, a middle man (or a proxy) cannot decipher the exchanged data packets.
When the client opens an SSL/TLS connection to the secure web server,
it verifies the server’s identity by checking two conditions: First, it checks whether
its certificate was signed by a CA known to the client. And second, it makes sure that
the common name (CN, also: host name) of the server matches the one it connects to.
If both conditions are true, the client assumes the connection is secure.
`Mitmproxy acts as a certificate authority`, however, not a very trustworthy one:
Instead of issuing certificates to actual persons or organizations,
Mitmproxy dynamically generates certificates to whatever hostname is needed for a connection.
If, for instance, a client wants to connect to https://www.facebook.com,
mitmproxy generates a certificate for “www.facebook.com” and signs it with its own CA.
Provided that the client trusts this CA, both of the above mentioned conditions are true
(trusted CA, same CN) - meaning that the client believes that the mitmproxy server is in fact
“www.facebook.com”.

* ## Mitmproxy as standard gateway (HTTP and HTTPS):

For both HTTP and HTTPS proxying, the server running mitmproxy must of course be able to intercept
the IP packets - meaning that it must be somewhere along the way of the packet path.
The easiest way to achieve this is to change the default gateway in the client
device to the mitmproxy server address.

* ## Trusted mitmproxy CA (HTTPS only):

For the HTTPS proxying to work, the client must know (and trust!) the mitmproxy CA,
i.e. the CA key file must be added to the trust store of the client.

### About Certificates:

## Introduction:
Mitmproxy can decrypt encrypted traffic on the fly, as long as the clients trusts its built-in
certificate authority.
Usually this means that the mitmproxy CA certificates have to be installed on the client device.

## Quick Setup

By far the easiest way to install the mitmproxy certificates is to use the built-in
certificate installation app. To do this, just start mitmproxy and configure your target
device with the correct proxy settings. Now start a browser on the device, and visit the
magic domain mitm.it. You should see something like this:
`mitmproxy`

Click on the relevant icon, follow the setup instructions for the platform you’re on
and you are good to go.

### Installing the mitmproxy CA certificate manually

Sometimes using the quick install app is not an option - Java or the iOS Simulator spring
to mind - or you just need to do it manually for some other reason.
Below is a list of pointers to manual certificate installation documentation for some common platforms.

### The mitmproxy certificate authority

The first time mitmproxy or mitmdump is run, the mitmproxy Certificate Authority (CA) is created
in the config directory (~/.mitmproxy by default).
This CA is used for on-the-fly generation of dummy certificates for each of the SSL sites that
your client visits. Since your browser won’t trust the mitmproxy CA out of the box,
you will see an SSL certificate warning every time you visit a new SSL domain through mitmproxy.
When you are testing a single site through a browser, just accepting the bogus SSL cert manually
is not too much trouble, but there are a many circumstances where you will want to configure
your testing system or browser to trust the mitmproxy CA as a signing root authority.
For security reasons, the mitmproxy CA is generated uniquely on the first start and is not
shared between mitmproxy installations on different devices.


## Installation on macOS

brew install mitmproxy

## Installation on Windows

The recommended way to install mitmproxy on Windows is to use the installer provided at mitmproxy.org.
After installation, you’ll find shortcuts for mitmweb (the web-based interface) and mitmdump
in the start menu. Both executables are added to your PATH and can be invoked from the command line.

## Installation on Linux

The recommended way to run mitmproxy on Linux is to use the pre-built binaries provided at releases.

## Installation on Arch Linux

sudo pacman -S mitmproxy

## Installation from Source on Ubuntu

sudo apt-get install python3-dev python3-pip libffi-dev libssl-dev
sudo pip3 install mitmproxy

On older Ubuntu versions, e.g., 12.04 and 14.04, you may need to install a newer version of Python.
mitmproxy requires Python 3.5 or higher. Please take a look at pyenv.
Make sure to have an up-to-date version of pip by running pip3 install -U pip.
