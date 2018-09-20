New API endpoints should use the v2 format.  See examples of other v2 endpoints for how to handle this.

Checklist for adding a new API endpoint:

* Register it in api/http.go
* Add it to the list of endpoints in api/csrf_test.go
* Write unit tests for it in the relevant api/*_test.go file. Tests must follow the example of existing tests.
* Add a method to the api.Client in api/client.go
* Add a stable integration test, if applicable
* Add a live integration test, if applicable
* Update api/README.md
* Update CHANGELOG.md