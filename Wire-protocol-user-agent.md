As of v25 a user agent string is sent in the wire protocol's introduction packet `INTR`.

This user agent is required for v25+ clients.

A skycoin user agent has the following format:

  `$NAME:$VERSION($REMARK)`

`$NAME` and `$VERSION` are required.

* `$NAME` is the coin or application's name, e.g. `Skycoin`. It can contain the following characters: `A-Za-z0-9\-_+`.
* `$VERSION` must be a valid [semver](http://semver.org/) version, e.g. `1.2.3` or `1.2.3-rc1`. Semver has the option of including build metadata such as the git commit hash, but this is not included by the default client.
* `$REMARK` is optional. If not present, the enclosing brackets `()` should be omitted.
  It can contain the following characters: `A-Za-z0-9\-_+;:!$%,.=?~ ` (includes the space character).

The v0.25.0 skycoin core implementation sends the following user agent: `skycoin:0.25.0`

An additional remark can be specified by the user with `--user-agent-remark`:

```sh
./run-client.sh --user-agent-remark=hello
```

The user agent will be `skycoin:0.25.0(hello)`.
