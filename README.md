# smserver-api

A Bash API for SMServer Android App

![Stable version](https://img.shields.io/badge/stable-1.0.0-blue.svg)
[![BSD-3 license](https://img.shields.io/badge/license-BSD--3--Clause-428F7E.svg)](https://tldrlegal.com/license/bsd-3-clause-license-%28revised%29)

*smserver-api* is a Bash library to centralize calls to [SMServer](http://github.com/cyosp/SMServer) Android App.
This library is composed of two files:
 * `/etc/smserver/smserver.conf.src`

	*The configuration file*

 * `/usr/lib/smserver/smserver.bash.src`

	*The library*

Both are located inside the directory **smserver-api** which allows to build a Debian package

# How To

## Configuration file

Configuration file is in fact a file which is sourced by the Bash library.

Thus syntax is the Bash syntax and configuration file `/etc/smserver/smserver.conf.src` is composed of:

| Name          | Meaning                      |
|:--------------|:-----------------------------|
| SMSERVER_HOST | SMServer host name or IP     |
| SMSERVER_PORT | SMServer port                |
| SMSERVER_DEST | Array with all phone numbers |

Example:

```bash
# SMServer host name or IP
SMSERVER_HOST="smserver"
# SMServer port
SMSERVER_PORT="8080"
# SMServer destination numbers
SMSERVER_DEST[0]="0123456789"
SMSERVER_DEST[1]="1234567890"
```

## Use

To use the library the only two steps are:

1. Source the library:
```bash
. /usr/lib/smserver/smserver.bash.src
```
2. Call the function to send a SMS:
```bash
callSMServer "My first SMS"
```

In this example *My first SMS* will be sent to phone numbers *0123456789* and *1234567890* calling [SMServer](http://github.com/cyosp/SMServer) Android App available on port *8080* and host *smserver*.

If there is at least one problem with one of the HTTP calls, function return *1*.

## Build a Debian package

You can build a Debian package with the following commands:

```bash
# Move to the repository directory
cd /github/smserver-api/repository/path
# Generate the Debian package
sudo dpkg-deb --build smserver-api
# Update the package name following Debian convention
VERSION=$(grep "Version" smserver-api/DEBIAN/control | cut -d ' ' -f 2)
ARCH=$(grep "Architecture" smserver-api/DEBIAN/control | cut -d ' ' -f 2)
mv smserver-api.deb smserver-api_${VERSION}_${ARCH}.deb
```

## Install the Debian package

These steps allows you to install [smserver-api](http://github.com/cyosp/smserver-api) Debian package:

```bash
# Move to the repository directory
cd /github/smserver-api/repository/path
# Install the latest Debian package version
sudo dpkg -i $(ls -mr smserver-api*.deb | cut -d ',' -f 1)
```

## Remove the Debian package

[smserver-api](http://github.com/cyosp/smserver-api) Debian package can be removed with this command line:

```bash
# Remove the Debian package
sudo apt-get remove smserver-api
```
