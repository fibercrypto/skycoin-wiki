All projects should use [Travis CI](http://travis-ci.com) to run tests before merging code.

At minimum the code should run `make check` on the latest two go versions in the default linux environment provided by Travis.

See the [Makefile standards](https://github.com/skycoin/skycoin/wiki/Skycoin-project-Makefile-standards) for a description of what `make check` should do.

Travis provides a "build badge" which can be included in the `README.md` to display the current build status. This should be included in the project's `README.md` below the title name.

The github repo's branch protection configuration must require the Travis status check to pass before merging any pull request. The branch protection configuration must also prevent pushing directly to the `master` or `develop` branch by anyone.

For golangci-lint, make sure you use this script for installing golangci-lint: https://github.com/skycoin/skycoin/blob/develop/ci-scripts/install-golangci-lint.sh
And invoke the script from the `.travis.yml` file, in the same way that skycoin does it, with a pinned version. See https://github.com/skycoin/skycoin/blob/develop/.travis.yml