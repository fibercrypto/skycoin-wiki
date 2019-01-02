Skycoin's Go projects should use [`dep`](https://github.com/golang/dep) to vendor their dependencies.  If the project is an application, i.e. it provides a `go run` command in the `cmd/` folder, the `vendor/` folder should be committed to the repo.

If the project is a library, the `vendor/` folder should **not** be committed to the repo.

Go has a new modules system for handling dependencies but it does not appear to be mature and `dep` has been working fine. In the future we may switch to using Go modules, although the Skycoin node/wallet itself will continue to use committed, vendored dependencies so that the source is entirely contained in one repository.

### Dep guide

[Installing `dep` for development](https://github.com/golang/dep/blob/master/docs/installation.md#development):

```sh
go get -u github.com/golang/dep/cmd/dep
```

`dep` vendors all dependencies into the repo.

If you change the dependencies, you should update them as needed with `dep ensure`.

Use `dep help` for instructions on vendoring a specific version of a dependency, or updating them.

When updating or initializing, `dep` will find the latest version of a dependency that will compile.

Examples:

Initialize all dependencies:

```sh
dep init
```

Update all dependencies:

```sh
dep ensure -update -v
```

Add a single dependency (latest version):

```sh
dep ensure github.com/foo/bar
```

Add a single dependency (more specific version), or downgrade an existing dependency:

```sh
dep ensure github.com/foo/bar@tag
```
