# General requirements

Skycoin is developed in Go and interfaces with some C libraries.

You will need:

* go1.4
* gcc or clang
* libgmp
* bash

There are 3 build version of Skycoin.  

* **skycoin** - The desktop GUI build, which includes a node-webkit wrapper around skycoind.  This introduces an extra build step.
* **skycoind** - The headless build.  An optional web interface is exposed, to run the GUI in the browser.
* **skycoindev** - The development version of the headless build.

Each version is the same code but with different configuration defaults.  The GUI also relies on the node-webkit prebuilt binaries.

# Platform instructions

* [Universal](#universal)
* [Linux specific](#linux)
* [OS X specific](#os-x)
* [Windows specific](#windows)

## Universal

### Setup

First, clone the repo:

```
git clone https://github.com/skycoin/skycoin
cd skycoin
```

Then, run the install script to set up the environment:

```
./setup.sh
```

This script does the following:

* Installs [gvm](https://github.com/moovweb/gvm), a go version manager.
* Installs go1.4 using `gvm`.
* Adds `gvm use go1.4` to `~/.bashrc`, if no `gvm use` instruction is already present in that file. This command sets $GOROOT and $GOPATH correctly.
* Activates go1.4 in the current environment with `gvm use go1.4`.
* Symlinks the skycoin repo directory into `$GOPATH/github.com/skycoin/` if not already present.  This is so that sub-package imports work correctly.  Unfortunately, Go enforces a strict project layout hierarchy in your working environment, making this step necessary.
* Installs skycoin dependencies with `go get -u`.  This will clone or update libraries in use by skycoin.  Dependencies are kept in `./compile/dependencies.txt`.  Note that there is no dependency versioning; go does not provide this functionality.  In the future we will have a versioned solution.

### Command line options

Skycoin can be configured with command line parameters.  Pass `-h` when running any of the builds to see a list of available options.

### Running skycoin, the GUI client

First you must build the client, because it has an additional compilation step due to the node-webkit wrapper.  Only rebuild if the skycoin go source changes.  Changes to the HTML/JS UI interface do not require rebuilding.

```
./gui.sh build
```

Then you may run it as often as you want.

```
./gui.sh run
```

### Running skycoind, the headless client

```
go run cmd/skycoind/skycoind.go
```

### Running skycoindev, the headless client with a development configuration

A wrapper script is provided for this, for convenience.

```
./run.sh
```

## Linux

The universal instructions should work unmodified with Linux.

## OS X

The universal instructions should work unmodified with OS X.  However, you may need to install some of the prerequisites with `brew`.

## Windows

The universal instructions should work unmodified in a `MingW` shell.


