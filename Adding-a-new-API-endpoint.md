New API endpoints should use the v2 format.  See examples of other v2 endpoints for how to handle this.

Checklist for adding a new API endpoint:

* Choose an existing, or create a new API set (see below for how to add an API set)
* Register it in `api/http.go`
* Add it to the list of endpoints in `api/http_test.go`
* Write unit tests for it in the relevant `api/*_test.go` file. Tests must follow the example of existing tests.
* Add a method to the `api.Client` in `api/client.go`
* Add a stable integration test, if applicable
* Add a live integration test, if applicable
* Update `api/README.md`
* Update `CHANGELOG.md`

Adding an API set:

* Define new API set name in `api/http.go`
* Add to `allAPISetsEnabled` in `api/http_test.go`
* Add to `allAPISets` (two instances) in `skycoin/config.go`
* Update documentation for the CLI options in `skycoin/config.go`
* Update documentation of API sets in `api/README.md`